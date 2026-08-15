# Manus–Gemini Import Workspace Index

> هذا هو الفهرس التشغيلي المركزي لمهمة **جيمناى امبورت** داخل مستودع **Knowledge-OS**، ويجمع بين مصدر التخطيط من Manus، ومواد `brain`، وخطط التنفيذ، والقرارات، والروابط التشغيلية.

## 1. هوية المهمة

| الحقل | القيمة |
|---|---|
| اسم المهمة | جيمناى امبورت — Gemini Import |
| مصدر التخطيط | Manus |
| المستودع | `saskw2010/Knowledge-OS` |
| حالة الفهرس | تأسيسي، بانتظار ملفات `brain` الأصلية |
| نقطة الدخول | هذا الملف |

## 2. أين نضع الملفات؟

| نوع المحتوى | المسار المقترح | طريقة التعامل |
|---|---|---|
| ملفات `brain` الأصلية | `imports/gemini-brain/raw/` | تحفظ دون تعديل |
| فهرس الملفات والبيانات الوصفية | `imports/gemini-brain/catalog/` | اسم الملف، النوع، المصدر، الحالة، والعلاقات |
| المعلومات المستخرجة | `imports/gemini-brain/knowledge/` | ملخصات وحقائق مرتبطة بمصادرها |
| الخطط التنفيذية | `imports/gemini-brain/plans/` | المراحل، الأولويات، المخرجات، ومعايير الإنجاز |
| القرارات والافتراضات | `imports/gemini-brain/decisions/` | قرارات مقترحة أو معتمدة مع سجل مصدرها |
| روابط العمل | `imports/gemini-brain/links/WORK-LINKS.md` | روابط Manus وGitHub وواجهات العرض |
| النتائج النهائية | `imports/gemini-brain/deliverables/` | وثائق جاهزة للمراجعة أو التسليم |

## 3. خريطة العمل

```text
Manus planning session
        ↓
imports/gemini-brain/raw/
        ↓
imports/gemini-brain/catalog/
        ↓
imports/gemini-brain/knowledge/
        ↓
imports/gemini-brain/plans/ + decisions/
        ↓
Knowledge-OS canonical documents / schemas / workstreams
        ↓
imports/gemini-brain/deliverables/
```

## 4. مكان رابط العمل

يوضع رابط جلسة Manus أو رابط المهمة في الملف:

`imports/gemini-brain/links/WORK-LINKS.md`

ويُفضّل أن يحتوي الملف على جدول منظم مثل الآتي:

| الاسم | الرابط | الغرض | الحالة |
|---|---|---|---|
| جلسة Manus | ألصق الرابط هنا | التخطيط والمراجعة | نشط |
| مستودع GitHub | `https://github.com/saskw2010/Knowledge-OS` | الملفات والإصدارات | نشط |
| واجهة GitHub Pages | `https://saskw2010.github.io/Knowledge-OS/` | القراءة والاستعراض البصري | نشط |
| مستكشف المستودع | `https://saskw2010.github.io/Knowledge-OS/repository.html` | فتح خريطة الملفات | نشط |

لا يوضع رابط Manus داخل ملفات المصدر الأصلية، بل في `WORK-LINKS.md` حتى تبقى المواد الأصلية مستقلة وقابلة للأرشفة.

## 5. من أين نفتح العمل؟

للمراجعة اليومية، افتح [هذا الفهرس](MANUS-GEMINI-IMPORT-WORKSPACE-INDEX.md) من GitHub. لمشاهدة المشروع كاملًا استخدم [Repository Explorer](repository-map.html). لقراءة وثائق Markdown في واجهة منسقة استخدم [Markdown Viewer](viewer.html). أما التعديلات والنسخ والإصدارات فتتم من المستودع الرئيسي على GitHub.

## 6. قواعد المصدر المرجعي

تظل قرارات Knowledge-OS المعتمدة وسجلات KDR والمخططات التنفيذية هي المرجع الأعلى. ملفات `brain` المستوردة تُعامل كمصادر ومواد إدخال، ولا تستبدل القرارات المعتمدة تلقائيًا. كل معلومة مستخرجة يجب أن تشير إلى الملف الأصلي، وكل اقتراح يجب أن يوسم بوضوح على أنه مقترح أو قيد المراجعة.

## 7. الخطوة التالية

بعد رفع مجلد `brain`، يبدأ العمل من `imports/gemini-brain/raw/`، ثم يُنشأ `catalog/FILE-CATALOG.md` وفهرس موضوعي داخل `knowledge/`. بعد ذلك تُحدّث الخطط والقرارات، وتُربط المخرجات بهذا الفهرس.
