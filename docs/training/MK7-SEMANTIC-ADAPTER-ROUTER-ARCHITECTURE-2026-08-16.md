# MK7 Semantic Adapter Router Architecture

## الخلاصة التنفيذية

المعمارية المقترحة هي منصة توسعة تعتمد على نموذج أساسي ثابت **Frozen Base Model**، مع Adapters متخصصة قابلة للإضافة، وسجل دلالي يصف قدرات كل Adapter، ثم Router يختار المسار المناسب.

```text
Model Files
    ↓
Adapter Manifest
    ↓
Semantic Registry
    ↓
Router
    ↓
Adapter / Tool / Plugin
```

## ما هو مثبت من البحث

- MixLoRA وMoLE وMiLoRA تثبت أن دمج عدة LoRA Experts مع Router اتجاه بحثي موجود.
- بعض الأنظمة تجعل كل Expert مسار LoRA مستقلًا، لكن تخصصه الوظيفي لا يظهر تلقائيًا من وجوده؛ يجب أن تدعمه البيانات أو التعريفات أو التدريب.
- Registry الدلالي ليس جزءًا إلزاميًا من كل نموذج؛ هو طبقة معمارية لإدارة النماذج والـAdapters والـTools والـPlugins.
- Router-R1 يدمج مصادر بيانات متعددة في Dataset واحدة لتدريب Router، ولا يربط تلقائيًا كل Dataset بـAdapter منفصل.

## تطبيق MK7 المقترح

| الطبقة | المسؤولية |
|---|---|
| Model Files | النموذج الأساسي، tokenizer، وملفات الأوزان |
| Adapter Manifest | هوية Adapter، الإصدار، المجال، اللغات، القدرات، Base Model |
| Semantic Registry | فهرس دلالي للقدرات والأمثلة والقيود ونتائج الاختبار |
| Router | اختيار Adapter أو أكثر، أو Tool/Plugin مناسب |
| Plugin Contract | شروط الإضافة والتحميل والتوافق والاختبار |
| Evaluation Gate | قبول الإضافة أو طلب تحديث Router |

## مثال Manifest

```json
{
  "name": "erp-adapter",
  "base_model": "gemma-3-270m-it",
  "domains": ["erp", "sales", "inventory"],
  "capabilities": ["sql", "tool_calling", "structured_json"],
  "languages": ["ar", "en"],
  "dataset_version": "mk7-v0.4",
  "status": "validated"
}
```

## دورة إضافة Adapter

1. إعداد Dataset متخصصة مع metadata واضحة.
2. تدريب Adapter فوق Base Model ثابت.
3. إنشاء Manifest والتحقق من توافقه.
4. تسجيله في Semantic Registry.
5. اختبار تحميل وجودة على 20–50 حالة.
6. اعتماده بدون إعادة تدريب Router إذا نجح التوجيه الدلالي.
7. تحديث أو تدريب Router فقط عند ظهور أخطاء اختيار متكررة أو إضافة مجال جديد.

## قرار معماري

إضافة Adapter جديدة **لا تتطلب تلقائيًا** إعادة تدريب Router. نبدأ بالـMetadata والوصف الدلالي والقواعد، ثم نرفع القرار إلى تدريب Router عند الحاجة. أما تحسين Adapter موجود بنفس المجال فلا يستلزم عادةً إعادة تدريب Router، لكنه يتطلب اختبار عدم التدهور.

## حدود الادعاء

المعمارية العامة موجودة في أبحاث Mixture-of-LoRA وMoE. مساهمة MK7 المحتملة ليست ادعاء اختراع فكرة MoE-LoRA، بل تطبيقها على العربية وERP وAgent/Coding، مع Semantic Registry وPlugin lifecycle وحواجز قياس تمنع Router collapse.

## مراحل التنفيذ

```text
P0  Contract + Manifest schema
P1  Registry ثابت بصيغة JSON
P2  Adapter واحد + Router تجريبي
P3  Adapterان مع بيانات منفصلة
P4  Semantic matching + قواعد توجيه
P5  Plugin hot-add مع اختبارات قبول
P6  Router training عند الحاجة فقط
```

## حالة الخطة

- **VERIFIED:** وجود نماذج بحثية لـLoRA Experts وRouters.
- **PARTIAL:** صلاحية ربط Adapter بالمجال عبر Metadata تحتاج اختبار MK7.
- **PLANNED:** Semantic Registry وPlugin Contract وEvaluation Gate.
- **UNRESOLVED:** هل يكفي التوجيه الدلالي دون تدريب Router على حجم بيانات MK7 النهائي.

## المراجع

- MixLoRA: https://arxiv.org/abs/2404.15159
- MiLoRA: https://arxiv.org/abs/2410.18035
- Mixture of LoRA Experts: https://arxiv.org/abs/2404.13628
- Router-R1: https://github.com/ulab-uiuc/Router-R1
- FlashRAG datasets: https://huggingface.co/datasets/RUC-NLPIR/FlashRAG_datasets
