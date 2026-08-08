# Sky365 Model Lab — MK-7 Training Handover

## 1. الهدف العام

المشروع يختبر بناء منظومة تدريب صغيرة قابلة لإعادة الاستخدام فوق Gemma 3 270M IT. الفكرة ليست إنشاء موديل كامل جديد لكل مهمة، بل إبقاء Base Model ثابتًا وإضافة Adapters صغيرة مستقلة لكل وظيفة.

المسار العام:

```text
User question
  -> Task / Adapter Router
  -> Base Gemma + selected LoRA Adapter
  -> deterministic FP32 inference
  -> task-specific parser/evaluator
```

## 2. ماذا أنجزنا؟

### مؤكد وناجح

- تم تحديد Base Model: `google/gemma-3-270m-it`.
- تم التحقق من سلامة أوزان Base: tensors finite، بلا NaN أو Inf.
- تم تثبيت Golden Runtime على Quadro P2000:
  - Python 3.11.9
  - PyTorch 2.6.0+cu124
  - Transformers 4.57.6
  - PEFT 0.20.0
  - FP32
- Dataset MK-7: 1000 سجل، مقسمة 800 تدريب / 100 Validation / 100 Test.
- تم تجميد Test وعدم استخدامها في التدريب أو اختيار checkpoint.
- Micro-overfit وIntent pipeline وLoRA save/reload نجحت.
- Controlled MK-7 training اكتمل حتى 6400 optimizer steps و8 epochs.
- أفضل Validation loss: `0.10316036139323842`.
- Test MK-7 lens accuracy: `38/40 = 95%`.
- تم إصلاح مشاكل parser/evaluator وcheckpoint/resume ومساحة القرص.
- تم إنشاء MKU targeted adapter مستقل، ونجح في تعلم الصيغة `8/8` بعد استخراج أول صيغة مكتملة.

## 3. الملفات الأساسية

```text
Base:
Q:\Colibri\models\google\gemma-3-270m-it

General MK-7 Knowledge Adapter:
Q:\Colibri\training\runs\mk7\v0.1\clean-20260808-153551\checkpoints\best-adapter

MKU specialized Adapter:
Q:\Colibri\training\runs\mk7\v0.2-mku-micro

HTML report:
Q:\Colibri\training\runs\mk7\v0.1\clean-20260808-153551\FINAL-REPORT.html
```

## 4. ما هو Base Model؟

الـBase Model هو Gemma الأصلي قبل تخصيص Sky365. يحتوي على:

- Transformer layers.
- Tokenizer.
- Token embedding matrix.
- Output/language-model head.
- Attention وMLP weights.
- المعرفة العامة التي تعلمها أثناء pretraining/instruction tuning.

نحن لا ننسخ Base إلى كل Adapter، ولا نعدّل ملفات Base الأصلية.

## 5. ما هي LoRA؟

LoRA لا تنسخ أوزان الموديل كاملة. بدلًا من تعديل مصفوفة كبيرة `W`، تضيف تعديلًا منخفض الرتبة:

```text
W_new = W_base + scale * B * A
```

حيث `A` و`B` مصفوفتان صغيرتان نسبيًا. لذلك:

- Base يظل ثابتًا.
- Adapter صغير.
- التدريب أسرع وأقل استهلاكًا للمساحة.
- يمكن تبديل Adapter دون إعادة تدريب Base.

في تجربتنا استهدف LoRA هذه الوحدات:

```text
q_proj, k_proj, v_proj, o_proj,
gate_proj, up_proj, down_proj
```

## 6. هل بنينا Embedding Array جديدة؟

لا.

في تدريبنا الحالي لم نضف Embedding Array جديدة ولم نعد تدريب Token Embeddings. الـEmbedding matrix الحالية تأتي من Base Model وتظل كما هي.

الذي أضفناه هو LoRA matrices صغيرة للوحدات المحددة أعلاه، وليس مصفوفة embedding جديدة.

هذا يعني:

```text
Token IDs
  -> Base Token Embeddings (unchanged)
  -> Transformer + LoRA deltas
  -> Base output head
```

لا يتم إنشاء embeddings جديدة عادةً إلا إذا:

- أضفنا Tokens جديدة إلى tokenizer.
- استخدمنا `resize_token_embeddings()`.
- استهدفنا طبقات embedding نفسها في LoRA.
- بنينا embedding model أو projection layer جديدًا.

لم يحدث أي من ذلك في MK-7.

## 7. الفرق بين أنواع Adapters

### Knowledge LoRA

يعلّم النموذج مفاهيم ومعرفة MK-7 وطريقة الإجابة عنها. المسار:

```text
Base + Knowledge LoRA
```

### Intent LoRA

مهمته تصنيف نية المستخدم إلى vocabulary محددة مثل `employee.create` أو `inventory.stock_query`. لا نخلطه مع Knowledge LoRA.

### Format / MKU LoRA

متخصص في إخراج صيغة أو عقد محدد، مثل:

```text
MKU = <O,E,R,M,A,T,H>
```

### Domain LoRA

Adapter لمجال محدد مثل ERP أو الأمن السيبراني أو الوثائق.

كل هذه Adapters تعديلات منفصلة فوق نفس Base، وليست موديلات كاملة مستقلة.

## 8. Adapter Switching

الـSwitching يعني تشغيل Adapter واحد في كل مرة حسب نوع السؤال:

```text
MK-7 question      -> Knowledge LoRA
Exact MKU formula  -> MKU LoRA
Intent routing     -> Intent LoRA
ERP question       -> ERP LoRA
```

ميزة هذا الأسلوب أنه واضح ويمنع تعارض التعليمات. وهذا هو الأسلوب الموصى به حاليًا.

## 9. هل يمكن Multi-Adapter؟

نعم، لكن ليس بمجرد جمع الملفات عشوائيًا.

الخيارات:

1. Adapter switching: اختيار Adapter واحد.
2. Adapter composition: تحميل أكثر من Adapter أو دمج تأثيراتهم، ويحتاج اختبار تعارض.
3. Weighted merge: دمج أوزان LoRA بأوزان محسوبة.
4. Router: موديل أو قواعد تختار Adapter المناسب.

MOE الحقيقي ليس مجرد Multi-LoRA. يحتاج Experts وRouter وقرار تدريب منفصل. لذلك المعمارية الحالية:

```text
Gemma Base
  + Adapter Router
      + Knowledge LoRA
      + MKU Format LoRA
      + Intent LoRA
      + Domain LoRAs
```

## 10. ما الذي لم ننجزه بعد؟

- لم ننشئ MOE حقيقي.
- لم ندمج Adapters معًا.
- لم نعدّل Base Model.
- لم نبنِ Embedding Array جديدة.
- لم نحوّل النموذج إلى GGUF بعد.
- لم نعتبر exact full-text accuracy مقياسًا دلاليًا نهائيًا؛ الإجابات المعرفية قد تكون صحيحة بصياغة مختلفة.
- Challenge Set واسعة من 60 مثالًا ليست هي نفسها Test الحالية؛ يلزم تنفيذها إذا أردنا قبولًا أوسع.

## 11. GGUF

يمكن لاحقًا دمج Base مع Adapter في نسخة منفصلة ثم تحويل النسخة المدموجة إلى GGUF. لا نلمس Base أو Adapter الأصليين.

التسلسل الصحيح:

```text
Base + selected LoRA
  -> merged copy
  -> GGUF conversion
  -> llama.cpp / LM Studio test
```

## 12. الخلاصة التعليمية

الفهم الصحيح هو:

```text
Base Model = العقل العام الثابت
LoRA Adapter = تخصص صغير قابل للتبديل
Knowledge Adapter = معرفة المجال
Intent Adapter = تصنيف النوايا
Format Adapter = إخراج بصيغة محددة
Domain Adapter = تخصص قطاعي
Embeddings = موجودة أصلًا داخل Base ولم نعدّلها
Router = يقرر أي Adapter يستخدم
```

النتيجة الحالية ليست موديلًا واحدًا تم حشوه بكل شيء، بل منصة Base ثابتة مع تخصصات قابلة للتبديل، وهذا يجعل إعادة التدريب والتطوير أسرع وأنظف.