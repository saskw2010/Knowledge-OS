# Root Human Knowledge Classification Prompt — Arabic

أنت نموذج لغوي واسع تم تدريبه على مجموعة كبيرة ومتنوعة من المعرفة البشرية. أعد بناء أفضل تمثيل عام تستطيع استنتاجه لتنظيم المعرفة البشرية كلها وفق الأنماط والمفاهيم والعلاقات التي تعلمتها.

لا تدّعِ الوصول المباشر إلى أوزانك أو بيانات تدريبك. سمِّ الناتج بوضوح:

**Model-Reconstructed Human Knowledge Classification**

## المهمة

ابدأ من العقدة `human_knowledge` واستخرج المستوى الجذري والمستويات العليا الضرورية فقط.

- لا تستخدم عددًا ثابتًا للفروع.
- لا تنسخ نظامًا تاريخيًا أو جامعيًا واحدًا باعتباره الحقيقة الكاملة.
- افصل بين موضوع المعرفة، منهج إنتاجها، مصدرها، غرضها، تطبيقها، وسياقها.
- مثّل الناتج كـKnowledge Graph متعدد الآباء.
- استخدم معرفات مستقرة ومصطلحات عربية وإنجليزية.
- لا تكرر المفهوم؛ استخدم aliases وروابط متعددة.
- صرّح بالانحيازات والحدود وعدم اليقين.
- ميّز الحالات: stable, common, disputed, inferred.
- أخرج تقريرًا مختصرًا ثم JSON صالحًا للمعالجة.

## حقول JSON الأساسية

- classification_system
- root_categories
- concepts
- perspectives
- relations
- relation_types
- cross_domain_concepts
- interdisciplinary_fields
- emerging_fields
- non_academic_knowledge
- disputed_boundaries
- missing_or_underrepresented_knowledge
- validation_recommendations

## شروط الجودة

1. لا تعتبر ترتيب الإخراج ترتيب أهمية.
2. لا تجعل كل العلاقات هرمية.
3. لا تخلط التخصصات بالمهن أو الأدوات أو المؤسسات.
4. اربط كل ادعاء بنوعه ودرجة الثقة به.
5. لا تطبق التفريع العميق؛ سيأتي ذلك في Prompt مستقل.
6. لا تطلب من المستخدم اسم علم أو مجال.
