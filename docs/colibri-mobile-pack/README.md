# Colibri — Mobile Reading Pack

آخر تحديث: 2026-08-13

الحالة: **Review pack — no Production deployment**

هذه نسخة قراءة للهاتف من أهم ملفات جلسة Colibri. لا تحتوي على نماذج أو
checkpoints أو أسرار. ملفات المصدر التشغيلية تظل في `Q:\Colibri`.

## ابدأ من هنا

1. [القرار التنفيذي الحالي](#القرار-التنفيذي-الحالي)
2. [مراجعة أمثلة MK7 v0.4](MK7-V04-REVIEW.md)
3. [قرار إعادة بناء MK7](MK7-ARCHITECTURE-RESET-DECISION.md)
4. [قرار نماذج الكود وLM Studio](CODING-GGUF-LMSTUDIO-DECISION.md)
5. [قرار NVIDIA Nemotron](NVIDIA-NEMOTRON-LOCAL-DECISION.md)

نسخ HTML الأصلية موجودة في المجلد نفسه:

- `CURRENT-EXECUTION-BOARD.html`
- `TRAINING-ARCHITECTURE-INFOGRAPHIC.html`
- `CHINA-HEALTH-AI-PARTNERSHIP-PLAN.html`
- `CHINA-HEALTH-AI-OUTREACH-PACK.html`
- `mk7-v0.4-review/review.html`

## القرار التنفيذي الحالي

### البيئات

| البيئة | المسار المحلي | الحالة | الاستخدام |
|---|---|---|---|
| Golden Training | `Q:\Colibri\training\venv-py311` | VERIFIED | مرجع FP32 LoRA، لا يُعدّل |
| Experimental Training Lab | `E:\Colibri-Environments\training-lab-py311` | VERIFIED | FP32، Adam8bit، NF4/QLoRA |
| LM Studio Runtime | `Q:\Users\mosta\.lmstudio\models` | VERIFIED INFERENCE | GGUF للاستدلال فقط |

### نتائج التدريب والتقييم

| المسار | النتيجة | القرار |
|---|---:|---|
| Intent FP32 LoRA | Fresh reload `100/100` | نجاح تقني على P2000 |
| Intent Adam8bit LoRA | Fresh reload `100/100` | 8-bit optimizer يعمل |
| Intent NF4/QLoRA + FP32 compute | Fresh reload `100/100` | QLoRA يعمل محليًا في النطاق المختبر |
| MK7 Gemma FP32 LoRA v0.3 | Macro-F1 `0.403` | NO-GO |
| MK7 constrained classifier | Macro-F1 `0.662` | أفضل مسار حالي، لكنه دون Gate `0.85` |
| GLM Edge 1.5B GGUF | Macro-F1 `0.050` | NO-GO |
| FunctionGemma diagnostic | Macro-F1 `0` | NO-GO |
| Coding GGUF | Phi-4 Mini أفضل المرشحين | PARTIAL؛ لا قرار تدريب بعد |

نجاح QLoRA أو Adam8bit هنا يثبت أن **التقنية تعمل** على الجهاز في الاختبار
المحدود، لكنه لا يثبت أن Dataset MK7 أو المهمة الدلالية نجحت.

## القرار المعماري لـMK7

```text
Input
  ↓
Unknown / MKU boundary
  ↓
Constrained multi-label classifier
  ↓
Deterministic JSON contract
  ↓
Optional LLM explanation after the decision
```

لا نستخدم LLM حرًا ليقرر العدسات ويولد JSON في خطوة واحدة. الدليل الحالي
أظهر أن الموديلات قد تتعلم التنسيق مع انهيار القرار الدلالي.

## ما يحتاج مراجعة المالك

### 1. MK7 v0.4

[افتح قائمة الأمثلة الـ62](MK7-V04-REVIEW.md). عند وجود خطأ، اكتب Comment
على السطر أو المثال داخل GitHub. لا يبدأ التدريب قبل اعتماد المجموعة.

### 2. China Health AI

المرشح الأول: **iFLYTEK Health**.

المسار الموازي: **Neusoft Healthcare**.

القصة المقترحة:

> Arabic Government Healthcare Operations Copilot

الـPilot الأول غير تشخيصي: اكتمال الملف، بحث موثق في السياسات، ورحلة المريض
الإدارية. حزمة الرسائل موجودة في HTML، ولم يتم إرسال أي شيء.

### 3. نقل النماذج

نقل OLMo وByT5 وmT5 إلى `H:` ما زال متوقفًا لأن القرص غير ظاهر. لم يتم حذف
أو نقل أي ملف.

## بوابات الأمان

- لا Router ولا Production قبل اجتياز MK7 Challenge والمراجعة البشرية.
- لا تدريب v0.4 قبل اعتماد الأمثلة وتجميد SHA-256.
- لا بيانات مرضى في China Sandbox؛ بيانات اصطناعية فقط.
- لا تحميل Nemotron على `Q:` الآن.
- لا تعتبر GGUF وزنًا تدريبيًا.

## سلامة النسخة

- هذه الحزمة للقراءة والمراجعة فقط.
- لا توجد كلمات مرور أو Tokens أو مفاتيح API مقصودة.
- الأرقام مأخوذة من artifacts المحلية، وليست توقعات نظرية.
