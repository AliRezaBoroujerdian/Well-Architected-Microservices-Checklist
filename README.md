# چک‌لیست معماری مناسب برای میکروسرویس‌ها
*Well-Architected Microservices Checklist*

*نسخه‌ی اول - ۱۴۰۴/۰۳/۱۲*
**علیرضا بروجردیان - کارشناس معماری نرم‌افزار**

## Operational Excellence
برتری عملیاتی

این بخش بر حوزه‌هایی تمرکز دارد که می‌توانند به بهبود عملیاتی میکروسرویس‌ها کمک کنند.

### Ownership
مالکیت

- مکانیزمی برای شناسایی مالکیت سرویس (توسعه و زمان اجرا) (مثلاً: استفاده از تگ‌ها (tags)، برچسب‌ها (labels) و غیره)
- نقاط تماس (Point of contacts) برای به‌روزرسانی‌ها و تغییرات
- فرایند و ماتریس (matrix) ارجاع (Escalation) 
- اعمال نمی‌شود

### Logging & Monitoring
ثبت لاگ و مانیتورینگ (پایش)

- برنامه (Application telemetry)
- کاربران (User activity telemetry)
- وابستگی‌ها (Dependency telemetry)
- تراکنش‌ها (Transaction traceability)
- اعمال نمی‌شود

### Observability
قابلیت مشاهده

- حجم کار (Workload health)
    - شناسایی KPIها و معیارها (Identify KPIs and metrics)
    - در دسترس بودن معیارهای پایه (Metrics Baseline available)
    - الگوهای ترافیک و معیارهای مورد انتظار (Expected traffic and metrics patterns)
    - بازنگری و تنظیم KPI و معیارها (Iterate KPI and Metrics review and adjustments)
    - ایجاد داشبورد و راه‌اندازی (Dashboard creation and on-boarding)
    - وضعیت درخواست/تراکنش HTTP، زمان و غیره در NewRelic یا Application Performance Monitoring وارد می‌شود.
    - سیاست‌های هشدار و کانال‌های اعلان در NewRelic یا APM تنظیم می‌شوند.
- مکانیزم ردیابی (Tracing mechanism) پیاده‌سازی شده است.

### Deployment/Rollback Automation
خودکارسازی استقرار و بازگشت به نسخه قبلی

- استفاده از کنترل نسخه (Version control)
- استراتژی‌های تست تعریف و اجرا شده‌اند
    - تست‌های بار موفقیت‌آمیز بوده‌اند
- خودکارسازی استقرار و بازگشت
    - قابلیت بازگشت (Rollback)
        - خودکار
        - فرایند دستی
- استراتژی‌های استقرار - Canary, Blue/Green و غیره
- استراتژی به‌روزرسانی‌های امنیتی
- زیرساخت غیرقابل تغییر (Immutable infra)
- مستندات مورد نیاز
- ملاحظات خاص محیطی (IIS، Kubernetes و …)

### Operational Readiness
آمادگی عملیاتی

- تایید معماری (Architecture Approval)
- تایید امنیت (Security Approval)
- انتشار در دایرکتوری سرویس (Published in service directory)
- مستندات API موجود (API documentation available)
- Runbook در دسترس
- Playbook در دسترس
- ظرفیت تیم عملیات (Operations team capacity)
- طراحی و اجرای یک شبیه‌سازی کنترل‌شده از شرایط بحرانی واقعی یا بار بالا (Game day plan and readiness)
    - خاموش کردن یکی از گره‌های دیتابیس برای تست Failover
    - ایجاد افزایش ترافیک ناگهانی برای مشاهده Auto-scaling
    - تأخیر مصنوعی در ارتباط بین میکروسرویس‌ها برای بررسی رفتار Retry
    - غیرفعال کردن یک وابستگی حیاتی برای بررسی میزان تاب‌آوری سیستم

## Security
امنیت

این بخش جنبه‌های امنیت معماری و زمان اجرا (runtime) یک سرویس مشخص را پوشش می‌دهد.

### Exposure
در معرض بودن

- سرویس خارجی (External service)
    - آیا جزو API Gateway است؟
- سرویس داخلی (Internal service)
    - آیا جزو Service Mesh است؟
- منطبق بر نیازمندی‌ها (Governance and Compliance)
- ریسک‌ها شناسایی شده‌اند

### Secure Operations
عملیات امن

- شناسایی و تعیین فرایند پایش مداوم آسیب‌پذیری امنیتی، به‌روزرسانی‌ها و رفع آنها
- شناسایی مدل تهدید (Threat Model)
- شناسایی هرگونه trade-off، ریسک در مقابل مزایا
- استفاده از Hardened Container Image تایید شده توسط InfoSec (تیم امنیت سازمان)
    - بسته‌های غیرضروری حذف شده‌اند.
    - تنظیمات پیش‌فرض ناامن اصلاح شده‌اند.
    - آسیب‌پذیری‌ها پچ (Patch) شده‌اند.
    - حداقل دسترسی‌ها اعمال شده‌اند.
    - ایمیج‌های اسکن‌شده و امن‌سازی‌شده توسط ابزارهایی مثل Anchore, Trivy, Clair
- نصب ابزارهای امنیتی مورد نیاز (agents/sidecars)

### IAM, Authentication and Authorization
مدیریت هویت، احراز هویت و مجوزدهی

- شناسایی مالکین سرویس و سطح دسترسی مورد نیاز
- استفاده از حداقل دسترسی (least-privilege) در roles and policies
- ایزوله کردن منابع با namespace (در اصطلاح Kubernetes) و دسترسی حداقلی به سرویس‌ها و نقش‌ها
- پیاده‌سازی روش‌های احراز هویت مثل Mutual TLS، JWT
- استفاده از ACL در صورت نیاز
- استفاده از گواهی‌نامه‌ها و اعتبارنامه‌های کوتاه‌مدت
- پیاده‌سازی گردش (rotation) اعتبارنامه‌ها و گواهی‌نامه‌ها

### Data-In-Transit
داده در حال انتقال

- استفاده از پروتکل‌ها و روش‌های رمزنگاری برای ارتباط همزمان (synchronous) سرویس به سرویس
- استفاده از پروتکل‌ها و روش‌های رمزنگاری برای ارتباط ناهمزمان (asynchronous) از طریق middleware، event hubs و غیره
- استفاده از سیاست‌های شبکه برای اجازه یا رد ارتباط سرویس‌ها

### Data at rest
داده در حالت استراحت

- استفاده از server-side encryption برای رسانه ذخیره‌سازی (media، service، filesystem)
- استفاده از رمزنگاری client side برای داده یا فیلدها در صورت نیاز
- فعال کردن رمزنگاری برای تمام سرویس‌های ابری استفاده شده (مانند پایگاه داده، صف‌ها، topicها، کش)
- پیاده‌سازی کنترل‌های اضافی برای داده‌های حساس طبق نیاز سازمان
- ذخیره اسرار و داده‌های حساس پیکربندی در مخزن جداگانه (در code repository نباشند.)
- اعمال سیاست‌های حداقل دسترسی برای دسترسی به مخزن اسرار
- اطمینان از اینکه مکانیزم پشتیبان‌گیری و بازیابی داده‌ها با سیاست‌های رمزنگاری و دسترسی سازمان مطابقت دارد

## Reliability
قابلیت اطمینان

تمرکز این حوزه تضمین اطمینان بهتر زمان اجرا برای سرویس‌ها و وابستگی‌های آنها است.

### Service Quotas
محدودیت‌های سرویس

- مرور محدودیت‌های سرویس و درخواست افزایش محدودیت نرم (soft-limit) در صورت نیاز. محدودیت‌های سخت (hard limits) باید در معماری در نظر گرفته شوند.
- دانستن و/یا ساخت مکانیزم پایش، هشدار و رفع محدودیت سرویس.

### Network Reliability
قابلیت اطمینان شبکه

- بهره‌گیری از اتصال شبکه با دسترس‌پذیری بالا. استفاده از backbone شبکه ارائه‌دهنده ابر هرجا ممکن باشد.
- تمام سیاست‌ها و قوانین شبکه زیرساختی (DNS، فایروال و غیره) پیکربندی شده باشند
- تمام سیاست‌ها و قوانین شبکه سطح سرویس (Service Mesh، Kubernetes Networking و غیره) پیکربندی شده باشند
- مکانیزم مدیریت شکست ارتباط سرویس پیاده‌سازی شده (مثلاً Throttling، تلاش‌های کنترل‌شده مجدد، تایم‌اوت‌ها، بک‌آف‌های نمایی، jitter)
- تست‌های شکست شبکه (network failure) انجام شده باشند.

### Request Processing
پردازش درخواست

- بررسی پردازش همزمان (Sync) در مقابل ناهمزمان (Async/Event-based)

### Reliable Deployment
استقرار قابل اطمینان

به بخش \"Deployment/Rollback Automation\" در \"Operational Excellence\" مراجعه شود.

### Scaling
مقیاس‌پذیری

- سیاست‌های مقیاس‌پذیری خودکار در سطح سرویس پیاده‌سازی شده‌اند
- عدم یا حداقل وابستگی همزمان سرویس به سرویس برای جلوگیری از coupling

### Data Backup and Recovery, Disaster Recovery
پشتیبان‌گیری داده و بازیابی، بازیابی از فاجعه

- برنامه و فرایند پشتیبان‌گیری و بازیابی داده موجود باشد.
- پشتیبان‌ها رمزنگاری شده باشند.
- برنامه اعتبارسنجی فرایند بازیابی.
- تعریف RTO - Recovery Time Objective و RPO - Recovery Point Objective
- استراتژی DR (بازیابی از فاجعه) موجود باشد

### Blast Radius
دامنه آسیب

- استقرار در چند مکان (Regions, Availability Zones و غیره)
- سرویس‌ها روی چند نود/ماشین/سرور در چند مکان اجرا شوند
- فرایند استقرار خودکار برای استقرار سریع در مناطق/دیتاسنترها در صورت نیاز
- مدیریت ظریف شکست در سطح کامپوننت (مثلاً سرویس ذخیره‌سازی یا صف به طور موقت در دسترس نباشد)
- تخصیص ظرفیت برای تحمل خطا
- چند نسخه کپی (replicas)

## Performance Efficiency
بهره‌وری عملکرد

داده‌های جمع‌آوری‌شده در اینجا می‌توانند به تصمیم‌گیری در مورد معماری، منابع و نحوه‌ی مقیاس‌پذیری کمک کنند.

### Identify Traffic/Load Patterns
شناسایی الگوهای ترافیک/بار

- کاربران فعال ماهانه/روزانه
- تراکنش‌ها/درخواست‌ها در ثانیه/دقیقه/ساعت/روز
- هر الگوی ترافیکی دیگر
- اندازه درخواست/پاسخ
- نیازمندی‌های حجم داده

### Resource Configuration Requirements
نیازمندی‌های پیکربندی منابع

- پیکربندی بهینه CPU، حافظه (Memory)، ذخیره‌سازی
- انتخاب storage type مناسب
- انتخاب database مناسب
- آیا نیاز به Node Affinity خاص (در اصطلاح Kubernetes) برای سخت‌افزار زیرساخت وجود دارد؟ (به طور مثال GPU، SSD و …)

### Performance Monitoring
مانیتورینگ عملکرد

- پیکربندی معیارها و ردیابی APM) Observability)
- پیکربندی Synthetics
- تنظیم هشدار برای نرخ خطا، p95، p99 و غیره

### Granularity
ریزساختار

- آیا سرویس به اندازه کافی ریزساختاری (granular) است تا نیازهای عملکرد و مقیاس‌پذیری را بر اساس الگوهای ترافیکی برآورده کند؟ (مثلاً قابل تنظیم برای خواندن در مقابل بار نوشتن)
- آیا سرویس باید ریزساختاری‌تر یا دانه‌ای‌تر (granular) شود؟

### Testing and Benchmarks
تست‌ها و بنچمارک‌ها

- آیا مقایسه‌های بنچمارک با سرویس‌های موجود برای تعیین نتایج عملکردی انجام شده یا مورد نیاز است؟
- تاخیر شبکه (Network Latency) لحاظ شده است.
- تست بنچمارک، نیازمندی‌ها را برآورده می‌کند.
- تست Load/Performance نیازمندی‌ها را برآورده می‌کند.

## Cost Optimization
بهینه‌سازی هزینه

اگر موارد زیر نادیده گرفته شوند، میکروسرویس‌ها ممکن است در ابتدا هزینه زمان اجرا بالایی نداشته باشند، اما در مقیاس می‌توانند به سرعت افزایش یابند.

### Consider The Dependencies
ملاحظات وابستگی‌ها

- انتخاب سرویس‌ها و معماری با هزینه بهینه
- محاسبه هزینه زمان اجرا برای همه سرویس‌های وابسته (شامل هزینه انتقال داده) مربوط به سرویس مورد نظر
- ملاحظه هزینه‌های مجوز

### Resource Identification
شناسایی منابع

- افزودن تگ‌ها و برچسب‌های مورد نیاز
- برآورده کردن نیازهای Showback/Chargeback
- برنامه‌ریزی برای شناسایی و بهینه‌سازی مستمر استفاده منابع
- فرایند شناسایی سرویس‌های غیرفعال/رها شده و حذف آنها

### Guardrails
محدودیت‌ها

- اعمال سیاست‌های سطح کلاستر برای محدود کردن استفاده منابع فراتر از حد مجاز
- شناسایی و اعمال حداقل/حداکثر منابع
- استثناها در صورت وجود

### Development & Build Environments
محیط‌های توسعه و ساخت

- استفاده از فرایندهای توسعه و تست محلی هرجا ممکن باشد

## Sustainability
پایداری

پایداری میکروسرویس‌ها از طریق تلاش مستمر برای اجرای بهینه و کافی سرویس‌ها به منظور برآورده کردن SLA و SLO حاصل می‌شود.

### بهینه‌سازی مستمر هزینه و بهبود استفاده از منابع

- پیکربندی CPU/Memory/Storage با حداقل ظرفیت برای برآورده کردن نیازهای درخواست/پاسخ/پردازش
- استفاده از سیاست‌های lifecycle (چرخه عمر) و automation(خودکارسازی) برای حذف منابع و داده‌های ناخواسته
- اجتناب از تخصیص بیش از حد منابع فراتر از نیازمندی‌های fault-tolerance
- استفاده از سرویس‌های مشترک و مدیریت‌شده هرجا ممکن باشد.

## Testing Strategy
راهبرد سنجش

یک زیرساخت قوی برای تست اطمینان می‌دهد که سرویس‌ها قابل اتکا، قابل نگهداری و در طول زمان قابل توسعه باشند.

### General Guidelines
راهنمایی‌های کلی

- آیا مستند استراتژی تست وجود دارد که دامنه و رویکرد را مشخص کند؟
- آیا مسئولیت‌های تست به وضوح تعریف شده‌اند (توسعه‌دهندگان، QA، DevOps)؟
- آیا خودکارسازی تست در CI/CD ادغام شده؟

### Unit Testing
تست واحد

- آیا همه واحدهای منطق کسب‌وکار با تست‌های خودکار پوشش داده شده‌اند؟
- آیا پوشش کد در سطح مناسب حفظ می‌شود؟
- آیا mocks/stubs به درستی برای ایزوله کردن واحدها استفاده شده؟

### Integration Testing
تست یکپارچه‌سازی

- آیا تعاملات کلیدی بین سرویس‌ها با تست‌های یکپارچه‌سازی آزمایش می‌شوند؟
- آیا محیط تست اختصاصی مشابه محیط تولید وجود دارد؟
- آیا وابستگی‌ها (مانند پایگاه داده، صف‌ها) با نمونه‌های واقعی یا کانتینری تست می‌شوند؟

### Contract Testing
تست قرارداد

- آیا contract test برای APIهای بین‌سرویسی وجود دارد (مثلاً با Pact)؟
- آیا نسخه‌بندی و سازگاری معکوس برای همه APIها تست شده؟
- آیا contract test خودکار و جزئی از CI/CD است؟

### End-to-End (E2E) Testing
تست انتها به انتها

- آیا جریان‌های حیاتی با تست E2E پوشش داده شده؟
- آیا تراکنش‌های مصنوعی برای اعتبارسنجی سلامت تولید استفاده می‌شوند؟
- آیا تست‌های E2E با داده‌های واقعی انجام می‌شوند؟

### Regression & Load Testing
تست بازگشتی و بار

- آیا مجموعه تست‌های بازگشتی به‌طور منظم به‌روزرسانی می‌شوند؟
- آیا تست‌های بار برای اعتبارسنجی عملکرد تحت فشار انجام می‌شوند؟
- آیا از ابزارهایی مانند JMeter، k6 یا Locust برای تست بار/عملکرد استفاده می‌شود؟

### Chaos & Resilience Testing
تست آشوب و تاب‌آوری

- آیا مهندسی آشوب برای تست تاب‌آوری سیستم اجرا می‌شود (مثلاً با Chaos Mesh، Gremlin)؟
- آیا مکانیزم‌های تزریق خطا برای شبیه‌سازی شکست شبکه یا سرویس وجود دارد؟

### Test Observability
قابلیت مشاهده تست

- آیا نتایج تست در داشبوردهای CI قابل مشاهده است؟
- آیا تست‌های ناپایدار (flaky tests) ردیابی و سریع حل می‌شوند؟
- آیا لاگ‌های تست‌های شکست‌خورده برای تحلیل بعد از حادثه ذخیره می‌شوند؟

## Data Management & Consistency
مدیریت داده و سازگاری

مدیریت درست داده‌ها در معماری توزیع‌شده بسیار حیاتی است. این بخش تضمین می‌کند که هر میکروسرویس مالک داده‌های خود باشد و مسئله سازگاری داده‌ها به دقت مد نظر قرار گیرد.

### General Principles
اصول کلی

- آیا هر میکروسرویس پایگاه داده مخصوص به خودش را دارد؟ (الگوی Database per Service)
- آیا از تماس‌های مستقیم بین پایگاه داده‌های سرویس‌ها خودداری شده است؟
- آیا مدل‌سازی داده‌ها با Bounded Context سرویس هماهنگ است؟

### Data Consistency
سازگاری داده‌ها

- آیا مفهوم سازگاری نهایی (Eventual Consistency) به روشنی پذیرفته و در صورت نیاز مدیریت می‌شود؟
- آیا از الگوهایی مثل SAGA Pattern یا Outbox Pattern برای مدیریت تراکنش‌های توزیع‌شده استفاده شده است؟
- آیا اقدامات جبرانی (Compensating Actions) برای تراکنش‌های بلندمدت یا شکست‌خورده تعریف شده است؟

### Data Sharing
اشتراک داده‌ها

- آیا سرویس‌ها از ارتباط غیرهمزمان (Asynchronous Communication) مانند رویدادها برای به اشتراک‌گذاری تغییرات وضعیت استفاده می‌کنند؟
- آیا استراتژی‌ای برای انتشار رویدادهای دامنه (Domain Events) مثل استفاده از پیام‌رسان‌هایی مانند Kafka یا RabbitMQ وجود دارد؟
- آیا مصرف‌کنندگان رویدادها Idempotent هستند (یعنی دریافت چندباره رویدادها مشکلی ایجاد نمی‌کند) و در برابر رویدادهای تکراری مقاوم‌اند؟

### Read Models & Querying
مدل‌های خواندن و پرس‌وجو 

-   آیا CQRS - Command Query Responsibility Segregation در مواقع لازم پیاده‌سازی شده است؟
-   آیا مدل‌های خواندن با مکانیزم‌های رویدادمحور (event-driven)‌ به‌روز می‌شوند؟
-   آیا از Materialized Views یا projections برای بهینه‌سازی خواندن داده استفاده می‌شود؟

### Data Ownership & Retention
مالکیت و نگهداری داده‌ها

-   آیا سیاست‌های نگهداری داده (retention policies) برای هر سرویس تعریف و اعمال شده است؟
-   آیا استراتژی‌های آرشیو و حذف داده‌ها با الزامات کسب‌وکار و قوانینی مثل GDPR منطبق است؟

### Data Security & Access
امنیت داده‌ها و دسترسی

-   آیا فیلدهای حساس داده‌ها در حالت استراحت (At Rest) و در انتقال (In Transit) رمزنگاری شده‌اند؟
-   آیا کنترل دسترسی سطح ردیف یا فیلد در صورت نیاز پیاده‌سازی شده است؟
-   آیا اسرار (مثل اعتبارنامه‌های دیتابیس، کلیدها) به صورت امن ذخیره می‌شوند؟ (مثلاً HashiCorp Vault یا AWS Secrets Manager)

### Schema Management
مدیریت اسکیمای داده

-   آیا schema migration با ابزارهای کنترل نسخه مانند EF Core Migrations ،Flyway یا Liquibase مدیریت می‌شوند؟
-   آیا تغییرات اسکیمای داده به‌صورت سازگار به عقب (Backward-compatible) بوده و تست شده‌اند؟
-   آیا برنامه‌ای برای برگشت به حالت قبلی (Rollback) در صورت شکست data migration وجود دارد؟
