# درس 1: تصویری‌سازی داده‌ها

## مقدمه

> "یک نمودار ساده بیش از هر چیز دیگری برای ذهن یک تحلیل‌گر داده‌، اطلاعات به ارمغان می‌آورد."  
> — جان توکی


نرم افزار R چندین چارچوب برای رسم نمودار دارد اما بسته ggplot2 یکی از زیباترین و انعطاف‌پذیرترین آن‌هاست. ggplot2 دستور زبان گرافیک را اجرا می‌کند، یک چارچوب منسجم برای توصیف و ایجاد نمودارهاست. با استفاده از ggplot2 شما در واقع با یادگیری یک چارچوب و به کارگیری آن در موقعیت‌های زیادی، می‌ةوانید بسیار سریع‌تر کارها را انجام دهید.

این فصل به شما یاد می‌دهد که چطور با استفاده از بسته ggplot2 داده‌هایتان را به تصویر بکشید. فصل را با ایجاد یک نمودار پراکندگی ساده شروع می‌کنیم و از آن برای معرفی نگاشت‌های ظاهری و اشیای هندسی - عناصری که اجزای بنیادین بسته ggplot2 هستند - استفاده خواهیم کرد. سپس شما را با نمایش توزیع‌ تک متغیرها و هم‌چنین نمایش روابط میان دو یا چند متغیر آشنا می‌کنیم. در نهایت با ذخیره کردن نمودارهایتان و ارائه نکاتی درباره رفع مشکلات احتمالی، فصل را به پایان می‌رسانیم.

### پیش‌نیازها

این فصل بر روی بسته ggplot2 - یکی از بسته‌های اصلی در مجموعه‌ی جامع tidyverse - تمرکز دارد. برای دسترسی به مجموعه‌ داده‌ها، صفحه‌های راهنما و توابع مورد استفاده در این فصل، بسته‌ی tidyverse را با اجرای دستور زیر بارگذاری کنید.

```{r}
library(tidyverse)
#> ── Attaching core tidyverse packages ───────────────────── tidyverse 2.0.0 ──
#> ✔ dplyr 1.1.0.9000 ✔ readr 2.1.4
#> ✔ forcats 1.0.0 ✔ stringr 1.5.0
#> ✔ ggplot2 3.4.1 ✔ tibble 3.1.8
#> ✔ lubridate 1.9.2 ✔ tidyr 1.3.0
#> ✔ purrr 1.0.1
#> ── Conflicts ─────────────────────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
31 You can eliminate that message and force conflict resolution to happen on demand by using the conflicted
package, which becomes more important as you load more packages. You can learn more about conflicted on
the package website.
2 Horst AM, Hill AP, Gorman KB (2020). palmerpenguins: Palmer Archipelago (Antarctica) penguin data. R
package version 0.1.0. https://oreil.ly/ncwc5. doi: 10.5281/zenodo.3960218.
#> ✖ dplyr::lag() masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all
#> conflicts to become errors
```

این خط کد، بسته‌ی جامع tidyverse را بارگذاری می‌کند، بسته‌هایی که تقریباً در هر تحلیل داده‌ای از آن‌ها استفاده خواهید کرد. هم‌چنین این خط به شما می‌گوید که کدام توابع بسته‌ی tidyverse با توابع پیش‌فرض موجود در R (یا سایر بسته‌هایی که از قبل ممکن است بارگذاری کرده باشید) تداخل دارند.

اگر این کد را اجرا و پیغام خطای 
<span dir="ltr">`there is no package called 'tidyverse'`</span>
 *(هیچ بسته‌ای با نام 'tidyverse' وجود ندارد)* دریافت کنید، شما باید ابتدا آن را نصب و سپس مجدداً دستور 
 <span dir="ltr">`library()`</span>
  را اجرا کنید:

```{r}
install.packages("tidyverse")
library(tidyverse)
```

شما تنها یک بار نیاز به نصب یک بسته دارید، اما باید آن را هر بار که نشست جدیدی را آغاز می‌کنید، بارگزاری کنید.

علاوه بر tidyverse، از بسته‌های palmerpenguins شامل مجموعه داده `penguins` که در خود اندازه‌گیری‌های بدنی پنگوئن‌ها در سه جزیره از جزایر پالمِر را دارد و ggthemes که یک پالت رنگی مناسب برای افراد دارای مشکل کور رنگی ارائه می‌دهد، استفاده خواهیم کرد.

```{r}
library(palmerpenguins)
library(ggthemes)
```

## نخستین گام‌ها

بیایید از اولین سوال خود استفاده کنیم تا بپرسیم: آیا پنگوئن‌ها با بال‌های بلندتر یا کوتاه‌تر وزن بیشتری دارند؟ احتمالاً در حال حاضر یک پاسخ دارید، اما سعی کنید پاسخ خود را دقیق کنید. رابطه بین طول بال و وزن بدن چگونه است؟ مثبت؟ منفی؟ خطی؟ غیرخطی؟ آیا پاسخ برای همه گونه‌های پنگوئن یکسان است؟

### دیتافریم `penguins`

می‌توانید فرضیه خود را با استفاده از دیتافریم `penguins` موجود در palmerpenguins (یعنی `palmerpenguins::penguins`) آزمایش کنید. یک دیتافریم مجموعه‌ای مستطیل شکل از متغیرها (در ستون‌ها) و مشاهدات (در سطرها) است. `penguins` شامل 344 مشاهده جمع‌آوری شده و در دسترس توسط دکتر کریستن گورمن و ایستگاه پالمر است.

برای دیدن راحت‌تر داده‌ها، می‌توانیم نام دیتافریم را در کنسول تایپ کنیم، که اطلاعات آن را چاپ می‌کند. توجه کنید که [344 × 8] به ما می‌گوید که این دیتافریم 344 سطر و 8 ستون دارد.

```{r}
penguins
```

این دیتافریم شامل 8 ستون است:

1. `species`: گونه پنگوئن (Adelie، Chinstrap یا Gentoo)
2. `island`: جزیره‌ای در archipelago پالمر، قطب جنوب (Biscoe، Dream یا Torgersen)
3. `bill_length_mm`: طول منقار (میلی‌متر)
4. `bill_depth_mm`: عمق منقار (میلی‌متر)
5. `flipper_length_mm`: طول بال (میلی‌متر)
6. `body_mass_g`: توده بدن (گرم)
7. `sex`: جنسیت پنگوئن (نر، ماده)
8. `year`: سال مطالعه (2007، 2008، یا 2009)

### هدف نهایی

هدف نهایی ما بازآفرینی تصویرسازی زیر است که رابطه بین طول بال و توده بدن این پنگوئن‌ها را نشان می‌دهد و در نظر می‌گیرد که گونه پنگوئن چیست.

```{r}
#| echo: false
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها، با خط مدل برازش شده
#|   برای هر گونه. نمودار نشان می‌دهد یک رابطه مثبت، تقریباً خطی بین این دو
#|   متغیر. همچنین نشان می‌دهد که این سه گونه (Adelie، Chinstrap و Gentoo) به
#|   خوبی از هم جدا می‌شوند بر اساس توده بدن و طول بال آن‌ها. پنگوئن‌های Gentoo
#|   بزرگتر از سایر دو گونه هستند.

ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point(aes(color = species, shape = species)) +
  geom_smooth(method = "lm") +
  labs(
    title = "توده بدن و طول بال",
    subtitle = "ابعاد برای پنگوئن‌های Adelie، Chinstrap و Gentoo",
    x = "طول بال (میلی‌متر)", y = "توده بدن (گرم)",
    color = "گونه", shape = "گونه"
  ) +
  scale_color_colorblind()
```

### ایجاد یک ggplot

بیایید گام به گام نمودار را بازآفرینی کنیم.

## ggplot() با

یک نمودار از تابع `ggplot()` شروع می‌شود که یک سیستم مختصات ایجاد می‌کند که می‌توانید لایه‌ها را به آن اضافه کنید. آرگومان اول `ggplot()` مجموعه داده‌ای است که در نمودار استفاده می‌شود، بنابراین `ggplot(data = penguins)` یک نمودار خالی ایجاد می‌کند.

```{r}
ggplot(data = penguins)
```

حالا می‌توانیم یک یا چند لایه به `ggplot()` اضافه کنیم. تابع `geom_point()` یک لایه از نقاط به نمودار شما اضافه می‌کند که یک نمودار پراکنش ایجاد می‌کند.

ggplot2 با بسیاری از توابع geom همراه است که هر کدام یک لایه متفاوت به یک نمودار اضافه می‌کنند.

هر تابع geom در ggplot2 یک آرگومان `mapping` می‌گیرد که تعریف می‌کند چگونه متغیرها در مجموعه داده شما به ویژگی‌های بصری (aesthetics) نگاشت می‌شوند. آرگومان `mapping` همیشه با `aes()` جفت می‌شود و آرگومان‌های `x` و `y` از `aes()` متغیرهایی را برای نگاشت به محورهای x و y مشخص می‌کنند.

```{r}
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها. نمودار نشان می‌دهد
#|   یک رابطه مثبت، تقریباً خطی بین این دو متغیر.

ggplot(data = penguins,
       mapping = aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point()
```

## افزودن aesthetics و لایه‌ها

### نگاشت aesthetics

بیایید چیزهای بیشتری به نمودار اضافه کنیم. یک راه برای افزودن متغیرهای اضافی به نمودار، نگاشت آن‌ها به یک aesthetic است. یک aesthetic یک ویژگی بصری از یکی از اشیاء در نمودار شماست. Aesthetics شامل چیزهایی مانند اندازه، شکل یا رنگ نقاط شما می‌شود.

می‌توانید اطلاعات درباره گونه پنگوئن را با نگاشت رنگ‌های نقاط به متغیر `species` نمایش دهید. برای دستیابی به این کار، aesthetic رنگ را درون `aes()` تنظیم می‌کنیم.

```{r}
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها. نمودار نشان می‌دهد
#|   یک رابطه مثبت، تقریباً خطی بین این دو متغیر. نقاط بر اساس گونه
#|   رنگ شده‌اند: نارنجی برای Adelie، سبز برای Chinstrap و آبی برای Gentoo.

ggplot(data = penguins,
       mapping = aes(x = flipper_length_mm, y = body_mass_g, color = species)) +
  geom_point()
```

وقتی یک متغیر پیوسته به رنگ نگاشت می‌شود، ggplot2 به طور خودکار یک مقیاس رنگ منحصر به فرد به هر مقدار منحصر به فرد از متغیر اختصاص می‌دهد، یک فرآیندی که scaling نامیده می‌شود. همچنین یک legend اضافه می‌کند که مقیاس هر سطح را توضیح می‌دهد.

بیایید یک aesthetic دیگر هم اضافه کنیم: `shape`. این باعث می‌شود هر گونه نه تنها رنگ متفاوتی داشته باشد، بلکه شکل متفاوتی هم داشته باشد.

```{r}
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها. نمودار نشان می‌دهد
#|   یک رابطه مثبت، تقریباً خطی بین این دو متغیر. نقاط بر اساس گونه
#|   رنگ (نارنجی برای Adelie، سبز برای Chinstrap و آبی برای Gentoo) و شکل
#|   (دایره برای Adelie، مثلث برای Chinstrap و مربع برای Gentoo) شده‌اند.

ggplot(data = penguins,
       mapping = aes(x = flipper_length_mm, y = body_mass_g, color = species,
                     shape = species)) +
  geom_point()
```

توجه کنید که legend به طور خودکار به‌روزرسانی شده است تا هم رنگ و هم شکل‌ها را نشان دهد.

و در نهایت، می‌توانیم یک منحنی smooth که رابطه بین توده بدن و طول بال را نشان می‌دهد اضافه کنیم.

```{r}
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها با یک خط smooth
#|   که رابطه بین این دو متغیر را نشان می‌دهد. نمودار نشان می‌دهد یک رابطه
#|   مثبت، تقریباً خطی بین این دو متغیر. نقاط بر اساس گونه رنگ و شکل شده‌اند.

ggplot(data = penguins,
       mapping = aes(x = flipper_length_mm, y = body_mass_g, color = species,
                     shape = species)) +
  geom_point() +
  geom_smooth(method = "lm")
```

ما موفق شدیم یک نمودار ایجاد کنیم که شامل لایه‌ها و aesthetic mappings است! اما هنوز با هدف نهایی ما که داشتیم مطابقت ندارد. در بخش بعدی labels را اضافه خواهیم کرد.

### برچسب‌ها (Labels)

می‌توانیم برچسب‌ها را با تابع `labs()` اضافه کنیم. بیایید عنوان نمودار را اضافه کنیم و برچسب‌های محور x و y را بهبود دهیم:

```{r}
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها با یک خط smooth
#|   که رابطه بین این دو متغیر را نشان می‌دهد. نمودار با "توده بدن و طول بال"
#|   عنوان شده و زیرنویس "ابعاد برای پنگوئن‌های Adelie، Chinstrap و Gentoo"
#|   دارد. محور x با "طول بال (mm)" و محور y با "توده بدن (grams)" برچسب گذاری
#|   شده‌اند. نقاط بر اساس گونه رنگ و شکل شده‌اند.

ggplot(data = penguins,
       mapping = aes(x = flipper_length_mm, y = body_mass_g, color = species,
                     shape = species)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(
    title = "توده بدن و طول بال",
    subtitle = "ابعاد برای پنگوئن‌های Adelie، Chinstrap و Gentoo",
    x = "طول بال (میلی‌متر)", y = "توده بدن (گرم)",
    color = "گونه", shape = "گونه"
  )
```

### طرح‌های رنگی و تم‌ها

یک مرحله آخر باقی مانده است: بهبود palette رنگی. بسته ggthemes پالت‌های رنگی مختلفی ارائه می‌دهد. یکی از آن‌ها scale_color_colorblind() است که برای افراد مبتلا به کوررنگی مناسب است.

```{r}
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها با یک خط smooth
#|   که رابطه بین این دو متغیر را نشان می‌دهد. نمودار با "توده بدن و طول بال"
#|   عنوان شده و زیرنویس "ابعاد برای پنگوئن‌های Adelie، Chinstrap و Gentoo"
#|   دارد. محور x با "طول بال (mm)" و محور y با "توده بدن (grams)" برچسب گذاری
#|   شده‌اند. نقاط بر اساس گونه رنگ و شکل شده‌اند. نمایش با palette رنگی
#|   دوستانه با کوررنگی است.

ggplot(data = penguins,
       mapping = aes(x = flipper_length_mm, y = body_mass_g, color = species,
                     shape = species)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(
    title = "توده بدن و طول بال",
    subtitle = "ابعاد برای پنگوئن‌های Adelie، Chinstrap و Gentoo",
    x = "طول بال (میلی‌متر)", y = "توده بدن (گرم)",
    color = "گونه", shape = "گونه"
  ) +
  scale_color_colorblind()
```

تبریک می‌گوییم! شما به هدف نهایی خود رسیدید و یک نمایش داده زیبا و اطلاعاتی ایجاد کردید!

## فراخوانی ggplot

وقتی ggplot codes شما طولانی‌تر می‌شوند، معمولاً خوب است که هر خط در یک خط جدید قرار بگیرد. همچنین بیشتر توصیه می‌شود که پیپ یا `+` را در انتهای هر خط قرار دهید و نه در ابتدای خط بعدی. این خواندن کد را آسان‌تر می‌کند.

```{r}
#| eval: false

ggplot(
  data = penguins,
  mapping = aes(x = flipper_length_mm, y = body_mass_g)
) +
  geom_point()
```

در بقیه کتاب، وقتی کد ggplot را می‌نویسیم، معمولاً نام آرگومان‌ها را حذف می‌کنیم تا کد ما موجزتر باشد. پس کد بالا به شکل زیر دوباره نوشته می‌شود:

```{r}
#| eval: false

ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point()
```

## تجسم روابط

برای تجسم یک رابطه باید حداقل دو متغیر را به حداقل دو aesthetics نگاشت کنیم. در مثال‌های بالا، ما دو متغیر پیوسته داشتیم (numeric) که آن‌ها را به موقعیت‌های x و y نگاشت کردیم. اما انواع بسیاری از geoms وجود دارند که می‌توانید از آن‌ها استفاده کنید برای تجسم انواع مختلف روابط، به خصوص متغیرهای categorical.

### یک متغیر numerical

برای تجسم توزیع یک متغیر numerical، می‌توانید از یک نمودار میله‌ای (bar chart) یا یک هیستوگرام استفاده کنید.

```{r}
#| warning: false
#| layout-ncol: 2
#| fig-alt: |
#|   دو نمودار - نمودار سمت چپ یک نمودار میله‌ای از فراوانی طول بال است و
#|   نمودار سمت راست یک هیستوگرام از توده بدن است. نمودار میله‌ای نشان می‌دهد
#|   که بیشترین فراوانی طول بال حدود 190 میلی‌متر است. هیستوگرام نشان می‌دهد
#|   که توزیع توده بدن تقریباً normal است با peak در حدود 3700-4000 گرم.

ggplot(penguins, aes(x = flipper_length_mm)) +
  geom_histogram(binwidth = 5)

ggplot(penguins, aes(x = body_mass_g)) +
  geom_histogram(binwidth = 200)
```

### یک متغیر categorical

برای تجسم یک متغیر categorical، می‌توانید از یک نمودار میله‌ای استفاده کنید.

```{r}
#| fig-alt: |
#|   نمودار میله‌ای از گونه. Adelie حدود 150 پنگوئن دارد، Chinstrap حدود 70
#|   و Gentoo حدود 120 پنگوئن دارد.

ggplot(penguins, aes(x = species)) +
  geom_bar()
```

از `fct_infreq()` می‌توان برای ترتیب میله‌ها بر اساس فراوانی استفاده کرد.

```{r}
#| fig-alt: |
#|   نمودار میله‌ای از گونه که بر اساس فراوانی مرتب شده است. Adelie با حدود
#|   150 پنگوئن بالاترین تعداد را دارد، سپس Gentoo با حدود 120، و Chinstrap
#|   با حدود 70 پنگوئن.

ggplot(penguins, aes(x = fct_infreq(species))) +
  geom_bar()
```

### یک متغیر numerical و یک categorical

برای تجسم رابطه بین یک متغیر numerical و یک categorical، می‌توانید از boxplots، violin plots و بسیاری گزینه‌های دیگر استفاده کنید.

```{r}
#| warning: false
#| layout-ncol: 2
#| fig-alt: |
#|   دو نمودار. نمودار سمت چپ boxplot از توده بدن در مقابل گونه است. نمودار
#|   سمت راست یک violin plot از توده بدن در مقابل گونه است. هر دو نمودار
#|   نشان می‌دهند که Gentoo بزرگترین پنگوئن‌ها را دارد و Chinstrap کوچکترین
#|   را دارد.

ggplot(penguins, aes(x = species, y = body_mass_g)) +
  geom_boxplot()

ggplot(penguins, aes(x = species, y = body_mass_g)) +
  geom_violin()
```

density plot نیز گزینه خوبی است:

```{r}
#| warning: false
#| fig-alt: |
#|   density plot از توده بدن گروه‌بندی شده با گونه و با رنگ‌های پر کننده
#|   مختلف برای هر گونه. نمودار نشان می‌دهد توزیع توده بدن برای هر گونه.

ggplot(penguins, aes(x = body_mass_g, color = species, fill = species)) +
  geom_density(alpha = 0.5)
```

### دو متغیر categorical

برای تجسم رابطه بین دو متغیر categorical، می‌توانید از geoms مانند `geom_count()` یا چهارگوش‌های گرمایی استفاده کنید.

```{r}
#| fig-alt: |
#|   نمودار پراکنش از گونه در مقابل جزیره. اندازه نقاط فراوانی را نشان می‌دهد.
#|   بزرگترین نقطه در Biscoe-Gentoo است و کوچکترین در Dream-Gentoo و
#|   Torgersen-Chinstrap.

ggplot(penguins, aes(x = island, y = species)) +
  geom_count()
```

### دو متغیر numerical

در حالی که نمودار پراکنش احتمالاً شناخته‌شده‌ترین نمودار برای تجسم رابطه بین دو متغیر numerical است، می‌توانید از geoms دیگر مانند `geom_hex()` و `geom_bin2d()` هم استفاده کنید.

```{r}
#| warning: false
#| layout-ncol: 2
#| fig-width: 4
#| fig-alt: |
#|   دو نمودار از توده بدن در مقابل طول بال. نمودار سمت چپ از شش‌گوش‌ها
#|   استفاده می‌کند و نمودار سمت راست از مستطیل‌ها. هر دو رنگ را برای نشان
#|   دادن تعداد نقاط در هر ناحیه استفاده می‌کنند.

ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_hex()

ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_bin2d()
```

### سه یا چند متغیر

همانطور که قبلاً دیدیم، می‌توانیم aesthetics (رنگ، شکل، اندازه و غیره) یا faceting را برای افزودن متغیرهای بیشتر به نمودار استفاده کنیم.

```{r}
#| warning: false
#| fig-alt: |
#|   نمودار پراکنش از توده بدن در مقابل طول بال پنگوئن‌ها. نقاط بر اساس
#|   گونه رنگ شده‌اند و نمودار به صورت facet شده است تا نمودارهای جداگانه‌ای
#|   برای هر جزیره نشان دهد.

ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point(aes(color = species, shape = island)) +
  facet_wrap(~island)
```

## ذخیره نمودارها

پس از اینکه نمودار خود را ساختید، احتمالاً می‌خواهید آن را خارج از R ذخیره کنید، به عنوان یک تصویر که می‌توانید در جای دیگری استفاده کنید. این کار با `ggsave()` انجام می‌شود.

```{r}
#| eval: false

ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point()

ggsave("penguin-plot.png")
```

## مشکلات رایج

همانطور که شروع به اجرای کد R می‌کنید، احتمالاً با مشکلات روبرو خواهید شد. نگران نباشید - همه این مشکل را دارند! نگارندگان این کتاب هر روز کد R می‌نویسند و هنوز هم به طور منظم اشتباهات می‌کنند.

یکی از مشکلات رایج این است که `+` باید در انتهای خط بیاید، نه ابتدای آن:

```{r}
#| eval: false

# این اشتباه است
ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g))
+ geom_point()
```

## تمرینها

1. چند سطر در دیتافریم `penguins` وجود دارد؟ چند ستون؟

2. متغیر `bill_depth_mm` در دیتافریم `penguins` چیست؟ به `?penguins` نگاهی بیندازید تا بیشتر بدانید.

3. یک نمودار پراکنش از `bill_depth_mm` در مقابل `bill_length_mm` بسازید. یعنی `bill_depth_mm` را روی محور y و `bill_length_mm` را روی محور x قرار دهید. این رابطه را توصیف کنید.

4. اگر یک نمودار پراکنش از `species` در مقابل `bill_depth_mm` بسازید چه اتفاقی می‌افتد؟ این نمودار چرا مفید نیست؟

5. چرا کد زیر خطا می‌دهد و چگونه می‌توانید آن را برطرف کنید؟

    ```{r}
    #| eval: false
    
    ggplot(data = penguins) +
      geom_point()
    ```

6. آرگومان `na.rm` در `geom_point()` چه کاری انجام می‌دهد؟ مقدار پیش‌فرض آرگومان چیست؟ یک نمودار پراکنش ایجاد کنید که در آن شما این آرگومان را به `TRUE` تنظیم می‌کنید.

7. یک برچسب به نمودار زیر اضافه کنید که می‌گوید "Data come from the palmerpenguins package."

    ```{r}
    #| eval: false
    
    ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
      geom_point(aes(color = bill_depth_mm)) +
      geom_smooth()
    ```

8. نمودار زیر را بازآفرینی کنید. چه aesthetic باید به چه متغیر نگاشت شود؟ آیا باید در سطح global یا در سطح geom تعریف شود؟

9. `geom_smooth()` را اجرا کنید. چه تفاوتی بین این دو نمودار است؟

    ```{r}
    #| eval: false
    
    ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
      geom_point() +
      geom_smooth()
    
    ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
      geom_point() +
      geom_smooth(aes(group = species))
    ```

10. این دو کد نمودار یکسانی تولید خواهند کرد یا نمودارهای متفاوت؟ چرا؟

    ```{r}
    #| eval: false
    
    ggplot(penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
      geom_point() +
      geom_smooth()
    
    ggplot() +
      geom_point(data = penguins,
                 aes(x = flipper_length_mm, y = body_mass_g)) +
      geom_smooth(data = penguins,
                  aes(x = flipper_length_mm, y = body_mass_g))
    ```

## خلاصه

در این فصل، شما اصول اساسی تجسم داده با ggplot2 را آموختید. ما با یک الگوی ساده شروع کردیم: `ggplot(data, aes(aesthetics)) + geom_*()`. سپس یاد گرفتید که چگونه:

- aesthetics را به متغیرها نگاشت کنید (مانند `x`، `y`، `color`، `shape`، و `size`)
- geoms مختلف را برای ایجاد انواع مختلف نمودارها استفاده کنید (`geom_point()`، `geom_bar()`، `geom_histogram()`، `geom_boxplot()`، و غیره)
- نمودارهای شما را با برچسب‌ها و طرح‌های رنگی سفارشی کنید
- نمودارهای خود را ذخیره کنید

در فصول بعدی، این مهارت‌ها را گسترش خواهیم داد و تکنیک‌های پیشرفته‌تری برای تجسم داده یاد خواهیم گرفت.
