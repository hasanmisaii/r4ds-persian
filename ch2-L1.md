# ۱ لایه‌ها

## مقدمه

در [فصل تصویرسازی داده‌ها]، یاد گرفتید که چگونه نمودارهایی با `ggplot()` ایجاد کنید. در آن فصل، من تلاش کردم تا در آسان‌ترین زمان ممکن نموداری به شما نشان دهم. اکنون زمان آن رسیده است که نحوه کار دستور کامل `ggplot()` را یاد بگیرید. ggplot2 مبتنی بر **دستور زبان گرافیک** است که مفهومی ایجاد شده توسط Leland Wilkinson. دستور زبان گرافیک راهی برای توصیف و ساخت انواع گسترده‌ای از آمارهای گرافیکی ارائه می‌دهد. ggplot2 این فلسفه کلی را با قرار دادن آن در یک بسته قابل اجرا در R پیاده‌سازی کرده است.

دستور زبان گرافیک به شما می‌گوید که یک نمودار آماری، نقشه‌ای از داده‌ها بر روی ویژگی‌های زیبایی‌شناختی (رنگ، شکل، اندازه) اشیاء هندسی (نقطه‌ها، خطوط، میله‌ها) است. نمودار همچنین ممکن است حاوی تبدیلات آماری داده‌ها باشد و بر روی یک سیستم مختصات مشخص ترسیم شود. Faceting می‌تواند برای تولید نمودارهای همان نوع برای زیرمجموعه‌های مختلف مجموعه داده استفاده شود. ترکیب این مؤلفه‌های مستقل تمام نمودارهای موجود در ggplot2 را تشکیل می‌دهد.

### پیش‌نیازها

این فصل روی ggplot2 تمرکز می‌کند که یکی از اعضای اصلی tidyverse است. برای دسترسی به مجموعه داده‌هایی که در این فصل استفاده می‌شوند، نیاز به نصب و بارگذاری بسته‌های زیر دارید:

```{r}
#| label: setup
library(tidyverse)
```

## الگوی ggplot

بیایید الگو را بازبینی کنیم که در [فصل تصویرسازی داده‌ها] یاد گرفتیم:

```{r}
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))
```

این کد یک نمودار تولید می‌کند، اما چطور؟ برای ساخت یک نمودار، ggplot2 داده‌ها را به ویژگی‌های زیبایی‌شناختی یک geom نقشه‌برداری می‌کند؛ ویژگی‌های بصری که می‌توانید نمودار را بر اساس آن تشخیص دهید. برای مثال، شما می‌توانید موقعیت نقاط، رنگ‌های آن‌ها، اندازه‌های آن‌ها، یا شکل‌هایشان را مشاهده کنید. شما این ویژگی‌ها را با نام‌گذاری آن‌ها به‌عنوان آرگومان‌های `aes()` کنترل می‌کنید. برای مثال، برای نقشه‌برداری ویژگی‌های x و y نقطه‌ها، `aes(x = displ, y = hwy)` استفاده می‌کنید. ggplot2 خود به خود مقیاسی برای نقشه‌برداری از هر ویژگی زیبایی‌شناختی به مجموعه‌ای از سطوح انتساب می‌دهد.

حالا می‌توانیم الگوی کلی کدی که برای ساخت یک نمودار با ggplot2 استفاده می‌کنید را تشخیص دهیم:

```r
ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(mapping = aes(<MAPPINGS>))
```

بقیه این فصل نشان می‌دهد که چگونه این الگو ساده را تکمیل و گسترش دهیم تا انواع مختلف نمودارها بسازیم. ما با `<GEOM_FUNCTION>` شروع می‌کنیم.

## لایه‌های هندسی

یک **geom** شیء هندسی است که یک نمودار برای نمایش داده‌ها استفاده می‌کند. افراد اغلب نوع نمودار را بر اساس نوع geom که استفاده می‌کند توصیف می‌کنند. برای مثال، نمودارهای میله‌ای از bar geoms استفاده می‌کنند، line charts از line geoms استفاده می‌کنند، boxplot ها از boxplot geoms استفاده می‌کنند، و غیره. Scatter plot ها شکسته قاعده هستند؛ آن‌ها از point geom استفاده می‌کنند. همان‌طور که در بالا می‌بینید، می‌توانید از geom های مختلف برای رسم همان داده‌ها استفاده کنید. نمودار سمت چپ از point geom استفاده می‌کند، و نمودار سمت راست از smooth geom، یک خط صاف برازش شده به داده‌ها استفاده می‌کند.

```{r}
# سمت چپ
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy))

# سمت راست
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

هر تابع geom در ggplot2 یک آرگومان mapping می‌گیرد. با این حال، هر زیبایی‌شناسی برای هر geom کار نمی‌کند. شما می‌توانید شکل یک نقطه را تنظیم کنید، اما شکل یک خط را نمی‌توانید. شما می‌توانید linetype یک خط را تنظیم کنید.

`geom_smooth()` یک نوع خط مختلف، `linetype`، را برای هر مقدار منحصر به فرد از متغیری که به آن نقشه‌برداری شده است، ترسیم می‌کند.

```{r}
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, linetype = drv))
```

اینجا `geom_smooth()` خطوط جداگانه‌ای برای خودروهایی با دریو‌های مختلف جلو، عقب، و چهارچرخ جداسازی می‌کند.

اگر این نظر را که نمودارهای مختلف ممکن است ویژگی‌های زیبایی‌شناختی مختلفی را نشان دهند جذاب می‌یابید، در نظر بگیرید که چه اتفاقی می‌افتد اگر شما `group` زیبایی‌شناسی را به یک متغیر categorical نقشه‌برداری کنید:

```{r}
ggplot(data = mpg) + 
  geom_smooth(mapping = aes(x = displ, y = hwy, group = drv))
```

ggplot2 یک شیء مجزا، در این مورد یک smooth line، برای هر مقدار منحصر به فرد از متغیر group ترسیم می‌کند. در عمل، ggplot2 خود به خود متغیرهای categorical را گروه‌بندی می‌کند، پس شما نیازی نیست که group aesthetic را به صراحت اضافه کنید اگر آن را به متغیری categorical نقشه‌برداری کرده‌اید. با این حال، group aesthetic مفید است زمانی که شما از یک متغیر categorical در ترکیب با multiple geoms استفاده می‌کنید.

برای نمایش multiple geoms در همان نمودار، multiple geom functions اضافه کنید:

```{r}
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(x = displ, y = hwy))
```

این کد هر دو point و smooth line را نمایش می‌دهد. با این حال، این کد تکراری است. شما mapping های یکسان را در دو جا تعریف کرده‌اید! برای جلوگیری از تکرار، می‌توانید mapping ها را برای `ggplot()` پاس کنید. ggplot2 این mapping ها را به‌عنوان global mappings در نظر می‌گیرد که برای هر لایه در نمودار اعمال می‌شوند. به عبارت دیگر، این کد همان نمودار قبلی را تولید می‌کند:

```{r}
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()
```

اگر شما mappings را در یک تابع geom قرار دهید، ggplot2 آن‌ها را به‌عنوان local mappings برای آن لایه در نظر می‌گیرد. این از global mappings فقط برای آن لایه استفاده می‌کند یا بر آن‌ها اولویت می‌یابد. این ممکن است mappings مختلفی را در لایه‌های مختلف نمایش دهد.

```{r}
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth()
```

شما می‌توانید از همان ایده برای تعیین data متفاوت برای هر لایه استفاده کنید. اینجا، smooth line فقط زیرمجموعه‌ای از داده‌ها، compact cars، را نمایش می‌دهد. Local data در `geom_smooth()` global data در `ggplot()` را برای آن لایه فقط جایگزین می‌کند.

```{r}
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point(mapping = aes(color = class)) + 
  geom_smooth(data = filter(mpg, class == "compact"), se = FALSE)
```

(از `geom_smooth()` و `se = FALSE` برای سرکوب نوارهای خطا استفاده کردیم؛ درباره آن‌ها بیشتر در [section on displaying uncertainty] یاد خواهیم گرفت.)

### تمرینها

۱. کدام geom برای ترسیم یک line chart استفاده می‌کنید؟ یک boxplot؟ یک histogram؟ یک area chart؟

۲. کد زیر را ذهنی اجرا کنید و پیش‌بینی کنید که چگونه خروجی به نظر می‌رسد. سپس آن را در R اجرا کنید و پیش‌بینی خود را بررسی کنید.

```{r}
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + 
  geom_smooth(se = FALSE)
```

۳. برای چه چیز `show.legend = FALSE` کار می‌کند؟ اگر آن را حذف کنید چه اتفاقی می‌افتد؟ فکر می‌کنید چرا من آن را در مثال‌های قبل استفاده کردم؟

۴. آرگومان `se` در `geom_smooth()` چه کاری می‌کند؟

۵. آیا این دو نمودار متفاوت به نظر می‌رسند؟ چرا/چرا نه؟

```{r}
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_point() + 
  geom_smooth()

ggplot() + 
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) + 
  geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
```

۶. کد R را برای بازسازی نمودارهای نشان داده شده در زیر بنویسید (اگرچه رنگ‌ها و شکل‌ها ممکن است متفاوت باشند):

![Six scatter plots arranged in a 2x3 grid](images/visualization-geoms-1.png)

## تبدیلات آماری

در ادامه، نگاهی به bar charts داشته باشیم. Bar charts ساده به نظر می‌رسند، اما جالب هستند زیرا برخی نکات جدید در مورد نمودارها را آشکار می‌کنند. در نظر بگیرید یک bar chart پایه، که با `geom_bar()` ترسیم شده است. نمودار زیر تعداد کل الماس‌ها را در مجموعه داده `diamonds` بر اساس `cut` نمایش می‌دهد:

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut))
```

محور x ویژگی‌های مختلف `cut` را نمایش می‌دهد، و محور y count یا تعداد را نمایش می‌دهد. اما انتظار! مجموعه داده `diamonds` متغیری برای count ندارد:

```{r}
diamonds
```

پس count از کجا می‌آید؟ بسیاری از نمودارها، مانند scatter plot، داده‌های خام شما را رسم می‌کنند. نمودارهای دیگر، مثل bar chart، یک مقدار جدید را برای رسم محاسبه می‌کنند:

* bar charts، histograms، و frequency polygons bin یا قطعه‌بندی داده‌ها را انجام می‌دهند و سپس تعداد نقاط موجود در هر bin را رسم می‌کنند.

* smooth lines مدلی را به داده‌ها برازش می‌دهند و سپس پیش‌بینی‌های مدل را رسم می‌کنند.

* boxplots خلاصه آماری قوی از توزیع را محاسبه می‌کنند و سپس نمایش خاصی از آن خلاصه را نمایش می‌دهند.

الگوریتمی که برای محاسبه مقادیر جدید یک نمودار استفاده می‌شود **stat**، مخفف transformation آماری، نامیده می‌شود. شکل زیر نشان می‌دهد که چگونه این فرآیند با `geom_bar()` کار می‌کند.

![Diagram showing the stat transformation process](images/visualization-stat-bar.png)

شما می‌توانید بیاموزید که کدام stat یک geom استفاده می‌کند با بررسی پیش‌فرض برای آرگومان `stat`. برای مثال، `?geom_bar` نشان می‌دهد که پیش‌فرض برای `stat` برابر "count" است، که به معنای آن است که `geom_bar()` از `stat_count()` استفاده می‌کند. `stat_count()` در مستندات برای `geom_bar()` مستند شده است، و اگر شما آن صفحه را به پایین بکشید، می‌توانید بخشی به نام "Computed variables" بیابید. که می‌گوید که دو متغیر جدید محاسبه می‌کند: `count` و `prop`.

شما معمولاً می‌توانید geoms و stats را به طور جایگزین استفاده کنید. برای مثال، شما می‌توانید نمودار قبلی را با استفاده از `stat_count()` به جای `geom_bar()` بازسازی کنید:

```{r}
ggplot(data = diamonds) + 
  stat_count(mapping = aes(x = cut))
```

این کار می‌کند زیرا هر geom یک stat پیش‌فرض دارد؛ و هر stat یک geom پیش‌فرض دارد.

سه دلیل وجود دارد که شما ممکن است بخواهید از یک stat به طور صریح استفاده کنید:

1. شما ممکن است بخواهید stat پیش‌فرض را تغییر دهید. در کد زیر، من stat از `geom_bar()` از count (default) به identity تغییر می‌دهم. این به من اجازه می‌دهد تا ارتفاع میله‌ها را به مقادیر خام در داده‌ها نقشه‌برداری کنم. متأسفانه زمانی که مردم در مورد bar charts صحبت می‌کنند، آن‌ها به طور خلاصه به این دو نوع نمودار کاملاً متفاوت اشاره می‌کنند. 

```{r}
demo <- tribble(
  ~cut,         ~freq,
  "Fair",       1610,
  "Good",       4906,
  "Very Good",  12082,
  "Premium",    13791,
  "Ideal",      21551
)

ggplot(data = demo) +
  geom_bar(mapping = aes(x = cut, y = freq), stat = "identity")
```

۲. شما ممکن است بخواهید پیش‌فرض mapping از متغیرهای تبدیل شده به aesthetics را override کنید. برای مثال، شما ممکن است بخواهید یک bar chart از نسبت‌ها نمایش دهید، نه counts:

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = stat(prop), group = 1))
```

برای پیدا کردن متغیرهایی که توسط stat محاسبه می‌شوند، مستندات را بررسی کنید.

۳. شما ممکن است بخواهید توجه بیشتری را به تبدیل آماری در کد خود جلب کنید. برای مثال، شما ممکن است از `stat_summary()` استفاده کنید، که متغیرهای x و y را برای هر مقدار منحصر به فرد x خلاصه می‌کند:

```{r}
ggplot(data = diamonds) + 
  stat_summary(
    mapping = aes(x = cut, y = depth),
    fun.min = min,
    fun.max = max,
    fun = median
  )
```

ggplot2 بیش از ۲۰ stat ارائه می‌دهد که برای استفاده با geoms استفاده می‌کنید. هر stat یک تابع است، بنابراین می‌توانید کمک دریافت کنید به شیوه معمول، مثل `?stat_bin`.

### تمرینها

۱. stat پیش‌فرض مرتبط با `geom_pointrange()` چیست؟ شما چگونه می‌توانید همان نمودار را با یک geom مختلف بازسازی کنید؟

۲. `geom_col()` چه کاری می‌کند؟ چطور با `geom_bar()` متفاوت است؟

۳. بیشتر geoms و stats در جفت‌ها می‌آیند که تقریباً همیشه همراه با هم استفاده می‌شوند. فهرستی از همه جفت‌ها بخوانید. آن‌ها چه قدر مشترک هستند؟

۴. متغیرهایی که `stat_smooth()` محاسبه می‌کند چیست؟ پارامترهایی که رفتار آن را کنترل می‌کنند چیست؟

۵. در گروه bar chart ما، باید `group = 1` تنظیم کنیم. چرا؟ به عبارت دیگر، مشکل این دو نمودار چیست؟

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, y = ..prop..))
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = color, y = ..prop..))
```

## تنظیمات موقعیت

یک جادوی دیگر مرتبط با bar charts وجود دارد. شما می‌توانید یک bar chart را با یک متغیر categorical دیگر رنگ کنید با استفاده از aesthetic `color` یا، مفیدتر، `fill`:

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, colour = cut))
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = cut))
```

توجه کنید که چه اتفاقی می‌افتد اگر شما fill aesthetic را به متغیر دیگری نقشه‌برداری کنید، مثل `clarity`: میله‌ها خود به خود stacked می‌شوند. هر مستطیل رنگی ترکیبی از `cut` و `clarity` را نمایش می‌دهد.

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity))
```

stacking به طور خودکار توسط **position adjustment** که با آرگومان `position` مشخص شده است، انجام می‌شود. اگر شما نمی‌خواهید stacked bar chart داشته باشید، می‌توانید از یکی از سه گزینه دیگر استفاده کنید: `"identity"`، `"dodge"` یا `"fill"`.

* `position = "identity"` هر شیء را دقیقاً در جایی که در context نمودار قرار می‌گیرد قرار می‌دهد. این برای bar charts خیلی مفید نیست، زیرا میله‌ها با یکدیگر همپوشانی می‌کنند. برای دیدن همپوشانی باید میله‌ها را کمی شفاف کنید با تنظیم `alpha` به یک مقدار کوچک، یا کاملاً شفاف کنید با تنظیم `fill = NA`.

```{r}
ggplot(data = diamonds, mapping = aes(x = cut, fill = clarity)) + 
  geom_bar(alpha = 1/5, position = "identity")
ggplot(data = diamonds, mapping = aes(x = cut, colour = clarity)) + 
  geom_bar(fill = NA, position = "identity")
```

position identity برای نمودارهای 2d مفیدتر است، مثل نقطه‌ها، که آن position پیش‌فرض است.

* `position = "fill"` مانند stacking کار می‌کند، اما هر مجموعه از stacked bars را به همان ارتفاع می‌رساند. این برای مقایسه نسبت‌ها در گروه‌ها آسان‌تر است.

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill")
```

* `position = "dodge"` اشیاء overlapping را مستقیماً در کنار هم قرار می‌دهد. این برای مقایسه مقادیر فردی آسان‌تر است.

```{r}
ggplot(data = diamonds) + 
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "dodge")
```

یک نوع دیگر از تنظیم وجود دارد که برای bar charts مفید نیست، اما برای scatter plots بسیار مفید است. به یاد بیاورید اولین نمودار ما را؟ آیا متوجه شدید که نمودار فقط ۱۲۶ نقطه نمایش می‌دهد، اگرچه در مجموعه داده ۲۳۴ مشاهده وجود دارد؟

![Scatter plot of highway fuel efficiency vs engine displacement](images/mpg-1.png)

مقادیر `hwy` و `displ` گرد شده‌اند بنابراین نقاط روی grid ظاهر می‌شوند و بسیاری از نقاط با یکدیگر همپوشانی می‌کنند. این مشکل **overplotting** شناخته می‌شود. این ترتیب باعث می‌شود تشخیص دهید که توزیع داده‌ها سخت باشد. آیا داده‌ها به طور یکنواخت در سراسر نمودار پخش شده‌اند، یا یک ترکیب غیرمعمول از `hwy` و `displ` وجود دارد؟

شما می‌توانید این gridding را با تنظیم position adjustment به "jitter" اجتناب کنید. `position = "jitter"` مقدار کوچکی از random noise به هر نقطه اضافه می‌کند. این نقاط را پخش می‌کند زیرا هیچ دو نقطه احتمالاً همان مقدار random noise را دریافت نخواهند کرد.

```{r}
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy), position = "jitter")
```

افزودن randomness به یک نمودار عجیب به نظر می‌رسد، اما در حالی که کمی accuracy در scale کوچک را قربانی می‌کند، revelation های زیادی در scale بزرگ حاصل می‌کند. چون این یک عملیات بسیار مفید است، ggplot2 یک shorthand برای `geom_point(position = "jitter")` ارائه می‌دهد: `geom_jitter()`.

برای یادگیری بیشتر درباره یک position adjustment، صفحه کمک مرتبط با هر adjustment را جستجو کنید: `?position_dodge`، `?position_fill`، `?position_identity`، `?position_jitter`، و `?position_stack`.

### تمرینها

۱. مشکل این نمودار چیست؟ چگونه می‌توانید آن را بهبود دهید؟

```{r}
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
  geom_point()
```

۲. پارامترهای `geom_jitter()` چیست؟ overplotting را کنترل می‌کنند؟

۳. `geom_jitter()` را با `geom_count()` مقایسه و تضاد دهید.

۴. position adjustment پیش‌فرض برای `geom_boxplot()` چیست؟ یک تصویرسازی از مجموعه داده mpg ایجاد کنید که آن را نشان دهد.

## سیستم‌های مختصات

سیستم‌های مختصات احتمالاً پیچیده‌ترین بخش ggplot2 هستند. سیستم مختصات پیش‌فرض سیستم مختصات Cartesian است که موقعیت‌های x و y به طور مستقل عمل می‌کنند تا موقعیت هر نقطه را پیدا کنند. گاهی اوقات مفید است از سیستم‌های مختصات دیگر استفاده کرد.

* `coord_flip()` محورهای x و y را عوض می‌کند. این مفید است (برای مثال)، اگر شما بخواهید horizontal boxplots داشته باشید. همچنین مفید است اگر شما برچسب‌های طولانی داشته باشید: سخت است آن‌ها را بدون همپوشانی روی محور x قرار دهید.

```{r}
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + 
  geom_boxplot()
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) + 
  geom_boxplot() +
  coord_flip()
```

* `coord_quickmap()` aspect ratio درستی برای maps تنظیم می‌کند. این زمانی مهم است که شما داده‌های فضایی رسم می‌کنید، که درباره آن در [Maps] یاد خواهید گرفت.

```{r}
nz <- map_data("nz")

ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black")

ggplot(nz, aes(long, lat, group = group)) +
  geom_polygon(fill = "white", colour = "black") +
  coord_quickmap()
```

* `coord_polar()` از مختصات polar استفاده می‌کند. مختصات Polar ارتباط جالبی بین bar chart و Coxcomb chart آشکار می‌کند.

```{r}
bar <- ggplot(data = diamonds) + 
  geom_bar(
    mapping = aes(x = cut, fill = cut), 
    show.legend = FALSE,
    width = 1
  ) + 
  theme(aspect.ratio = 1) +
  labs(x = NULL, y = NULL)

bar + coord_flip()
bar + coord_polar()
```

### تمرینها

۱. یک stacked bar chart را به pie chart تبدیل کنید با استفاده از `coord_polar()`.

۲. `labs()` چه کار می‌کند؟ مستندات را بخوانید.

۳. تفاوت بین `coord_quickmap()` و `coord_map()` چیست؟

۴. نمودار زیر چه چیزی درباره رابطه بین شهر و highway mpg به شما می‌گوید؟ چرا `coord_fixed()` مهم است؟ `geom_abline()` چه کار می‌کند؟

```{r}
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point() + 
  geom_abline() +
  coord_fixed()
```

## دستور زبان لایه‌ای گرافیک

در قسمت‌های قبلی، شما درباره بخش‌های مختلف یک نمودار یاد گرفتید: data، aesthetic mappings، geometric objects، statistical transformations، position adjustments، و coordinate systems. الگوی قالب الگوی قابل گسترش که اجازه می‌دهد شما هر نموداری را بسازید:

```r
ggplot(data = <DATA>) + 
  <GEOM_FUNCTION>(
     mapping = aes(<MAPPINGS>),
     stat = <STAT>, 
     position = <POSITION>
  ) +
  <COORDINATE_FUNCTION> +
  <FACET_FUNCTION>
```

در الگوی ما، ما هفت پارامتر داریم که **دستور زبان لایه‌ای گرافیک** را تعین می‌کنند. به طور خلاصه، یک نمودار آماری یک نقشه‌برداری از data به aesthetic attributes (color، shape، size) از geometric objects (points، lines، bars) است. نمودار ممکن است حاوی statistical transformations از داده‌ها باشد و بر روی یک coordinate system خاص ترسیم شود. Faceting می‌تواند برای تولید نمودارهای همان نوع برای زیرمجموعه‌های مختلف از مجموعه داده استفاده شود. ترکیب این seven parameters یک نمودار منحصر به فرد می‌سازد.

این دستور زبان ما را قادر می‌سازد تا به طور مختصر طیف گسترده‌ای از نمودارها را توصیف کنیم، از basic scatterplot گرفته تا complex multi-layered graphics.

## خلاصه

این فصل آموخت که چگونه دستور زبان گرافیک، فلسفه کامنی که زیر ggplot2 قرار دارد. شما یاد گرفتید که بیشتر نمودارها از همان مولفه‌های زیربنایی ساخته شده‌اند: یک data set، یک coordinate system، و geoms—نمایش‌های بصری نقاط داده.

برای افزودن پیچیدگی به نمودارهای خود، شما فقط لایه‌های اضافی اضافه می‌کنید. در این فصل، شما درباره یک لایه یاد گرفتید، geometric object، اما در فصل‌های آینده، شما درباره سایر انواع لایه‌ها یاد خواهید گرفت، از جمله statistical transformations و position adjustments.

به گرامر که یاد گرفته‌اید فکر کنید: شما می‌توانید data را به aesthetic attributes از یک geom نقشه‌برداری کنید. این فرمول ساده امکان ایجاد نمودارهای پیچیده و چندلایه از building blocks ساده را فراهم می‌کند. در فصل‌های بعدی، ما گسترش خواهیم داد که چه data types ای را می‌توانید تصویرسازی کنید و چه techniques ای را می‌توانید برای آماده‌سازی داده‌های خود برای تصویرسازی استفاده کنید.