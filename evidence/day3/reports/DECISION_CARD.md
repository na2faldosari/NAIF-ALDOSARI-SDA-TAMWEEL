# بطاقة قرارك — Tamweel Lite

**الحالة:** جاهزة للمراجعة؛ لا تعني اعتمادًا أو درجة
**مصدر الأرقام:** LIVE · **الاستراتيجية:** weighted · **الصفوف:** 5,039 OOF

**المهمة:** الفئة الموجبة `default_within_90d=1` تعني حدث تعثر اصطناعي خلال90 يومًا بعد الطلب. كل طية تحقق طلبات لاحقة، وتستبعد عملاءها من التدريب وتشترط نضج نتيجة التدريب قبل بدايتها. المعرّفات والتاريخ خارج المدخلات.

| الدليل | القيمة |
|---|---:|
| العتبة المقيدة، بالقيمة الكاملة | 0.6583471436014694 |
| عتبة أقل خسارة دون قيد | 0.44863935722081005 |
| Recall | 40.89% |
| Precision | 29.85% |
| AP مجمع منOOF | 0.3100 |
| الإشارات | 526 من 5,039 |
| FN / FP | 227 / 369 |
| الخسارة التعليمية | 2639 وحدة |
| خسارة0.5 | 2403 وحدة؛ ضمن السعة: False |
| التغير عن0.5 | +236 وحدة؛ الموجب زيادة |
| خسارة لكل10,000 طلب، تطبيع حسابي | 5237.15 وحدة |
| فجوة معدل الإنذار الخاطئ بين المناطق | 0.648 نقطة مئوية |

**القاعدة:** درجة ≥ 0.6583471436014694 تعني إشارة مراجعة داخل التمرين؛ غير ذلك بلا إشارة. لا تتخذ موافقة أو رفض تمويل حقيقي. احفظ الدقة الكاملة؛ تقريب العتبة قد يغيّر حجم الطابور.

**السياسة:** FN=10 وFP=1 وحدات تعليمية، وسعة 12% لكل فترة بعد التقريب لأسفل. ليست ريالات فعلية أو رسوم أدوات أو خصمًا من الدرجة.

**دليل السعة:** الفترة 1: 137/195, الفترة 2: 183/200, الفترة 3: 206/207.

## لماذا اخترت هذه العتبة؟
A cutoff of 0.65835 (rounded) gave the lowest simulated loss among cutoffs that stayed within the 12% review limit in every OOF period. The busiest period had 206 flags for 1,733 applications.

## الخسارة والسعة
The cutoff flagged 526 of 5,039 applications for 2,639 loss units. At 0.5 it flagged 1,005 for 2,403 units, but the busiest period reached 23.20%. With 10 units per missed default and 1 per false flag, fitting the queue cost 236 additional units in this run.

## فرق المناطق وما يحتاج إلى مراجعة
The observed FPR ranged from 7.64% in other (93/1,217) to 8.29% in western (96/1,158). The 0.65 percentage point gap is descriptive. Check the denominators and see whether the pattern persists in later periods.

## حدود النتيجة
The sweep selected and summarized the cutoff on the same 5,039 OOF development rows. Earlier warm-up rows lack OOF scores, the model scores are uncalibrated, and the 10:1 cost and 12% cap are teaching assumptions.

OOF تغطي 50.39% من التدريب و100% من الصفوف المؤهلة؛ 4,961 صفًا تمهيديًا بلا تنبؤ. اختيار العتبة وتقدير خسارتها هنا يستخدمان أهدافOOF نفسها؛ هذه نتيجة تطوير لا اختبار نهائي. لم نستخدم التحدي. المقارنة الجغرافية وصفية وليست شهادة عدالة، والأوزان لا تضمن معايرة الدرجات.

## سؤالك الأول: لماذا قد تخدعكAccuracy؟
Defaults make up 7.62% of the OOF rows. Flagging none would give 92.38% accuracy with zero recall and would miss every default.

## سؤالك الثاني: لماذا تختار علىOOF؟
The forward OOF probabilities were calculated for applications unseen by each fitting fold. Training predictions would reward fitting the same rows. The challenge batch has no labels and was reserved for the final scored export.

أدلتك في `artifacts/threshold_metrics.json` و`day3_period_capacity.csv` و`day3_region_audit.csv` و`day3_cost_sensitivity.csv` و`cost_curve.png`. الحساسية سيناريوهات ±20% لخسارةFN، وليست فترات ثقة. راجع السعة والمعايرة عند تغير البيانات؛ لا تفترض ثباتهما مستقبلًا.
