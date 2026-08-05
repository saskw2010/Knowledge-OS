# Knowledge-OS Execution Plan v1.0

## Human Knowledge Classification Pipeline

## الهدف

بناء **Meta-Knowledge Graph** عالمي يوحّد أنظمة تصنيف المعرفة البشرية دون إلغاء أي نظام أو طمس اختلافاته التاريخية أو المؤسسية أو المنهجية.

النظام لا يفترض أن المعرفة شجرة واحدة، بل يتعامل معها كشبكة متعددة المناظير، مع الحفاظ على المصدر، والغرض، والسياق، ودرجة الثقة لكل عقدة وعلاقة.

---

## المرحلة 1 — جمع أنظمة التصنيف

نجمع كل نظام كما هو، مع توثيق اسمه، ومصدره، وغرضه، وتاريخه، وإصداره، ورخصة استخدام بياناته إن وجدت.

أمثلة أولية:

- Aristotle
- Al-Farabi
- Ibn Khaldun
- Francis Bacon
- Auguste Comte
- UNESCO ISCED-F
- OECD FORD / Frascati
- Dewey Decimal Classification
- Library of Congress Classification
- OpenAlex Topics
- Scopus ASJC
- Web of Science Categories
- GPT model-reconstructed view
- Gemini model-reconstructed view
- Claude model-reconstructed view

كل واحد منها يُخزَّن ككيان مستقل من النوع:

```text
Classification System
```

ولا يُعامل على أنه Knowledge Graph نهائي أو مرجع وحيد.

---

## المرحلة 2 — تطبيع كل نظام

لكل نظام نستخرج بنيته الأصلية:

```text
Root Categories
    ↓
Categories
    ↓
Subcategories
    ↓
Leaves
```

### قواعد التطبيع

- الحفاظ على الأكواد الرسمية والأسماء الأصلية.
- إضافة ترجمة عربية وإنجليزية دون استبدال الاسم الرسمي.
- عدم تغيير الأبوة الأصلية داخل النظام.
- تسجيل إصدار النظام وتاريخ الاستخراج.
- تسجيل مصدر كل عقدة.
- تسجيل ما إذا كانت العقدة رسمية، تاريخية، مولدة، أو مستنتجة.

---

## المرحلة 3 — إنشاء Classification Nodes

كل بند داخل أي نظام يصبح عقدة من النوع:

```text
Classification Node
```

مثال:

```text
OECD FORD
└── Natural Sciences
    └── Mathematics
```

العقدة هنا تمثل **موضعًا داخل نظام بعينه**، وليس المفهوم العالمي الموحّد للرياضيات.

الحد الأدنى لبيانات العقدة:

- `classification_node_id`
- `classification_system_id`
- `official_code`
- `name_ar`
- `name_en`
- `parent_node_ids`
- `child_node_ids`
- `source_reference`
- `version`
- `status`

---

## المرحلة 4 — توسيع كل Node

كل Classification Node تمر على عملية توسعة باستخدام Prompt ثابت وقابل للتكرار.

الناتج المقترح:

- Definition
- Scope Includes
- Scope Excludes
- Parent
- Official Children
- Model-Inferred Children
- Related Concepts
- Prerequisites
- Methods
- Skills
- Tools
- Applications
- Historical Context
- Cross References
- Sources
- Confidence
- Validation Status

### قاعدة حاسمة

لا يُسمح للموديل بتعديل الشجرة الرسمية.

يجب فصل:

```text
Official Children
```

عن:

```text
Model-Inferred Children
```

وبذلك لا تختلط المؤسسة الرسمية باستنتاج النموذج.

---

## المرحلة 5 — إنشاء Canonical Concepts

بعد استخراج وتوسيع العقد من الأنظمة المختلفة، نبدأ بناء المفاهيم الموحّدة.

بدل إنشاء مفاهيم منفصلة مثل:

```text
OECD Mathematics
UNESCO Mathematics
Dewey Mathematics
GPT Mathematics
```

ننشىء مفهومًا واحدًا:

```text
Canonical Concept: Mathematics
```

ثم نربط جميع العقد التصنيفية به.

الحد الأدنى لبيانات المفهوم:

- `concept_id`
- `canonical_name_ar`
- `canonical_name_en`
- `aliases`
- `definition`
- `concept_type`
- `classification_node_ids`
- `source_ids`
- `confidence`
- `validation_status`

---

## المرحلة 6 — بناء Crosswalks

نربط كل عقدة تصنيفية بالمفهوم الموحّد المناسب.

مثال:

```text
OECD Mathematics ──classified_as──> Mathematics
Dewey 510 ──classified_as─────────> Mathematics
ISCED Mathematics ──classified_as─> Mathematics
GPT View Mathematics ──classified_as──> Mathematics
```

### حالات المطابقة

كل مطابقة تُصنّف كواحدة من الحالات التالية:

- Exact Match
- Close Match
- Broad Match
- Narrow Match
- Partial Overlap
- Historical Equivalent
- Disputed Match
- No Safe Match

ولا تُجبر العقد المختلفة على التطابق عند غياب الدليل.

---

## المرحلة 7 — بناء علاقات الـGraph

المعرفة لا تُمثّل بعلاقات الأب والابن فقط.

العلاقات الأساسية تشمل:

- `broader_than`
- `narrower_than`
- `part_of`
- `has_part`
- `subfield_of`
- `has_subfield`
- `depends_on`
- `prerequisite_for`
- `overlaps_with`
- `applies`
- `uses_method`
- `uses_tool`
- `studies`
- `produces`
- `supports`
- `contrasts_with`
- `historically_emerged_from`
- `classified_as`
- `equivalent_to`
- `related_to`

كل علاقة يجب أن تحمل:

- المصدر
- المنظور
- الاتجاه
- التفسير
- درجة الثقة
- حالة التحقق

---

## المرحلة 8 — Validation

كل Concept وكل Relation تمر بعملية تحقق متعددة المصادر.

مصادر التحقق المقترحة:

- المصدر الرسمي لنظام التصنيف
- مصادر أكاديمية موثوقة
- GPT
- Gemini
- Claude
- نماذج Teacher أخرى
- خبير بشري عند الحاجة

### نتائج التحقق

- Verified
- Supported
- Plausible
- Disputed
- Rejected
- Needs Human Review

### حساب الثقة

درجة الثقة لا تعتمد على عدد الموديلات فقط، بل على:

- قوة المصدر
- استقلال المصادر
- حداثة المصدر
- تطابق التعريفات
- وضوح الحدود
- وجود خلاف علمي
- المراجعة البشرية

---

## المرحلة 9 — Dataset Generation

لا تتحول أي عقدة إلى بيانات تدريب قبل اجتياز بوابة الجودة.

المخرجات الممكنة:

- Question Answering
- Flashcards
- Multiple Choice Questions
- Explanations
- Summaries
- Comparison Tasks
- Classification Tasks
- Relation Prediction
- Tool Calling
- Structured Extraction
- Evaluation Sets
- Safety and Uncertainty Tasks

كل عينة تدريب يجب أن تحمل Provenance وتربط بالمفاهيم والمصادر التي أُنشئت منها.

---

## المرحلة 10 — Teacher and Distillation Pipeline

بعد اعتماد البيانات:

```text
Validated Knowledge Graph
        ↓
Teacher Dataset
        ↓
Teacher Model Evaluation
        ↓
Distillation Dataset
        ↓
Sky365 Tiny Models
```

لا يُستخدم الـGraph فقط لتوليد التدريب، بل أيضًا لبناء اختبارات مستقلة تقيس:

- الدقة
- الاتساق
- تغطية المفاهيم
- فهم العلاقات
- الاعتراف بعدم اليقين
- احترام اختلاف المناظير

---

## اللوب التكراري لكل نظام

```text
Select Classification System
        ↓
Import Official Structure
        ↓
Create Classification Nodes
        ↓
Expand Each Node
        ↓
Normalize Names and Aliases
        ↓
Detect Duplicates
        ↓
Resolve Canonical Concepts
        ↓
Create Crosswalks
        ↓
Generate Relations
        ↓
Validate
        ↓
Queue Children
        ↓
Stop at Depth, Quality, or Budget Condition
```

---

## شروط التوقف

عملية التوسعة لا تستمر بلا حدود.

تتوقف عند تحقق واحد أو أكثر من الشروط التالية:

- الوصول إلى عمق محدد.
- انخفاض الثقة تحت الحد المقبول.
- غياب مصادر كافية.
- العقدة أصبحت دقيقة أكثر من حاجة المشروع.
- تكرار المفهوم مع عقدة موجودة.
- اكتشاف دورة غير مبررة.
- تجاوز ميزانية التوليد أو المراجعة.
- الحاجة إلى خبير بشري.

---

## القاعدة الذهبية

نبدأ من **أنظمة التصنيف**، ثم نبني المفاهيم الموحّدة.

```text
Classification Systems
        ↓
Classification Nodes
        ↓
Canonical Concepts
        ↓
Crosswalks
        ↓
Meta-Knowledge Graph
        ↓
Validated Datasets
        ↓
Teacher Models
        ↓
Tiny Models
```

---

## مبدأ المصدر والمنظور

كل معلومة في Knowledge-OS يجب أن تحتفظ بمصدرها ومنظورها.

لا نقول فقط:

> الرياضيات من العلوم الشكلية.

بل نسجل مثلًا:

- وفق نظام فلسفي معين: العلوم الشكلية.
- وفق OECD FORD: موضع بحثي محدد.
- وفق ISCED-F: مجال تعليمي محدد.
- وفق Dewey: رقم فهرسة محدد.
- وفق نموذج GPT: تصنيف مولّد يحتاج إلى تحقق.

وهذا المبدأ هو الذي يحوّل النظام من قاعدة بيانات موحدة بالقوة إلى **Meta-Knowledge Graph متعدد المناظير وقابل للتدقيق**.

---

## أول Pilot مقترح

يبدأ التنفيذ بنظام **OECD FORD** للأسباب التالية:

- رسمي وموثق.
- صغير نسبيًا مقارنة بأنظمة المكتبات.
- متعدد المستويات لكنه قابل للإدارة.
- مناسب لاختبار الـschemas والـCrosswalks.
- يمكن مقارنته لاحقًا بـISCED-F وOpenAlex.

### مخرجات الـPilot

1. سجل كامل لنظام FORD.
2. جميع الجذور الرسمية.
3. Classification Nodes للمستويين الأول والثاني.
4. توسيع محدود للعقد.
5. Canonical Concepts أولية.
6. Crosswalk تجريبي مع ISCED-F.
7. تقرير جودة ومشكلات.
8. Dataset تجريبي صغير بعد التحقق.

---

## تعريف النجاح للنسخة الأولى

تُعد النسخة الأولى ناجحة عندما تستطيع المنظومة:

1. استيراد نظام تصنيف رسمي دون تشويه بنيته.
2. توسيع عقدة مع فصل الرسمي عن المستنتج.
3. اكتشاف أن عقدتين من نظامين مختلفين تشيران إلى مفهوم واحد.
4. الحفاظ على الاختلاف عندما لا تكون المطابقة دقيقة.
5. إنتاج Graph قابل للتتبع إلى مصادره.
6. توليد Dataset صغير قابل للمراجعة من المفاهيم المعتمدة.
