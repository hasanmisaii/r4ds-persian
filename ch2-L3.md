# ۳ ارتباط

## مقدمه

در [تحلیل اکتشافی داده‌ها], شما یاد گرفتید که چگونه از نمودارها برای درک داده‌هایتان استفاده کنید. در آن فصل، تمرکز بر روی سرعت کشف بود: شما در حال ساختن نمودارهای بسیاری به سرعت بودید تا درک بهتری از داده‌هایتان پیدا کنید. اکنون زمان آن رسیده که بر روی **ارتباط** متمرکز شوید، یعنی ایجاد نمودارهایی که ایده‌هایتان را به وضوح و مؤثر به دیگران منتقل کند.

هنگامی که برای ارتباط نمودار می‌سازید، هدف شما کمک به مخاطب در درک سریع و دقیق آنچه می‌خواهید نشان دهید است. این کاری است که کمی تلاش و ظرافت می‌خواهد. در این فصل، تکنیک‌هایی را برای بهبود نمودارهایتان جهت ارتباط یاد خواهید گرفت.

### پیش‌نیازها

در این فصل دوباره بر روی ggplot2 تمرکز می‌کنیم. همچنین از مقداری dplyr برای دستکاری داده‌ها و چند بسته ggplot2 extension استفاده خواهیم کرد، از جمله **ggrepel** و **viridis**. به جای بارگذاری این بسته‌ها جداگانه، ما scales بسته‌ای را بارگذاری می‌کنیم که تابع‌های مفیدی برای سفارشی کردن توقف‌ها و برچسب‌ها فراهم می‌کند.

```{r}
#| label: setup
#| message: false

library(tidyverse)
library(scales)
library(ggrepel)
```

## برچسب

شروع خوبی برای تبدیل یک نمودار اکتشافی به یک نمودار تفسیری، افزودن برچسب‌های خوب است. برچسب‌ها را با تابع `labs()` اضافه می‌کنید:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = class)) +
  geom_smooth(se = FALSE) +
  labs(
    title = "کارایی سوخت کاهش می‌یابد با افزایش اندازه موتور",
    subtitle = "دو نفره‌ها (ورزشی) استثنا هستند چون سبک و دارای موتور بزرگ هستند",
    caption = "داده از fueleconomy.gov",
    x = "اندازه موتور (لیتر)",
    y = "کارایی سوخت در بزرگراه (مایل در گالن)",
    colour = "نوع خودرو"
  )
```

هدف یک عنوان جذب توجه به یافته اصلی است. عنوان‌های خوب توصیفی هستند:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = class)) +
  geom_smooth(se = FALSE) +
  labs(
    title = "دو نفره‌ها دارای کارایی سوخت غیر منتظره‌ای هستند",
    subtitle = "دو نفره‌ها (ورزشی) استثنا هستند چون سبک و دارای موتور بزرگ هستند",
    caption = "داده از fueleconomy.gov",
    x = "اندازه موتور (لیتر)",
    y = "کارایی سوخت در بزرگراه (مایل در گالن)",
    colour = "نوع خودرو"
  )
```

اگر نیاز به افزودن متن بیشتری دارید، دو گزینه مفید وجود دارد:

1. می‌توانید زیرعنوان را کمی طولانی‌تر کنید. در `ggplot()`, زیرعنوان با فونت کوچک‌تر نمایش داده می‌شود تا جزئیات بیشتری در مورد یافته اصلی ارائه دهد.

2. می‌توانید متن اضافی را در caption اضافه کنید. Caption با فونت کوچک‌تری در گوشه پایین راست نمایش داده می‌شود و اغلب برای توضیح منبع داده‌ها استفاده می‌شود.

همچنین ممکن است بخواهید برچسب‌های محورها را جایگزین کنید. معمولاً ایده خوبی است که نام متغیر را با توضیح کاملی جایگزین کنید و واحد اندازه‌گیری را اضافه کنید.

برچسب‌ها را با قرار دادن آن‌ها در `\n` در چندین خط تقسیم کرد نیز ممکن است. یک راه دیگر این است که از `str_wrap()` استفاده کنید تا متن‌های طولانی را به خودکاری شکسته کنید.

### تمرینات

1. یک نمودار ایجاد کنید و برچسب‌های مختلفی را آزمایش کنید. چگونه `title`, `subtitle` و `caption` استفاده می‌کنید؟

2. بسته `ggrepel` چه کاری انجام می‌دهد؟ یک مثال از استفاده از آن بسازید.

3. آرگومان `hjust` و `vjust` در `geom_text()` چه کاری انجام می‌دهند؟

## حاشیه‌نویسی

علاوه بر برچسب‌گذاری مؤلفه‌های اصلی نمودارتان، اغلب مفید است که مشاهدات فردی یا گروه‌هایی از مشاهدات را حاشیه‌نویسی کنید. ابزار اولیه برای این کار `geom_text()` است که متنی را در نمودار اضافه می‌کند. geom_text() مشابه geom_point() است، اما یک ویژگی زیبایی‌شناختی اضافی دارد: `label`. این کار آن را انعطاف‌پذیر می‌کند: شما می‌توانید متن را در مکان‌هایی با داده‌ها اضافه کنید، و همچنین متنی را در جاهای دلخواه نمودار اضافه کنید.

دو نوع برچسب وجود دارد که احتمالاً می‌خواهید روی نمودارتان قرار دهید:

1. برچسب‌های منفرد که یک نقطه خاص را شناسایی می‌کنند. این برچسب‌ها معمولاً با فیلتر کردن داده‌ها اضافه می‌شوند.

2. برچسب‌هایی که کل گروه‌هایی از نقاط را شناسایی می‌کنند. اینها می‌توانند با تجمیع داده‌ها و سپس اضافه کردن برچسب‌ها به نقاط تجمیع شده ساخته شوند.

اجازه دهید مثالی از هر دو را ببینیم. ابتدا، بیایید خودروهایی را که بیشترین کارایی سوخت را دارند برچسب‌گذاری کنیم:

```{r}
best_in_class <- mpg %>%
  group_by(class) %>%
  filter(row_number(desc(hwy)) == 1)

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_text(aes(label = model), data = best_in_class)
```

این کار کرد، اما برچسب‌ها سخت خوانده می‌شوند چون آن‌ها درست روی نقاط نشسته‌اند. می‌توانیم آن‌ها را کمی بالا ببریم با `vjust`:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_text(aes(label = model), data = best_in_class, vjust = "bottom", hjust = "right")
```

این بهتر است، اما هنوز خوندنی‌اش سخت است چون برچسب‌ها روی هم قرار گرفته‌اند. ما می‌توانیم از بسته **ggrepel** استفاده کنیم. این بسته مجموعه‌ای از geoms مفید ارائه می‌دهد که برچسب‌ها را به خودکاری دور از روی هم قرار می‌دهند. در اینجا، ما از `geom_text_repel()` استفاده می‌کنیم:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_text_repel(aes(label = model), data = best_in_class)
```

مفید است که یادآوری کنیم که می‌توانید همان ایده را برای سایر geoms استفاده کنید. به‌عنوان مثال، می‌توانید از `geom_label()` به جای `geom_text()` استفاده کنید اگر می‌خواهید مستطیل‌های محتوایی رسم کنید. همچنین بسته **ggrepel** `geom_label_repel()` ارائه می‌دهد.

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_label_repel(aes(label = model), data = best_in_class)
```

همچنین می‌توانید برچسب‌ها را در گوشه‌های نمودار قرار دهید. اینجا قراردادی وجود دارد که `x` و `y` را به `Inf` و `-Inf` تنظیم کنید. در این مثال، من `hjust` و `vjust` را به کنترل ترازیب برچسب تنظیم کردم:

```{r}
class_avg <- mpg %>%
  group_by(class) %>%
  summarise(
    displ = median(displ),
    hwy = median(hwy),
    .groups = "drop"
  )

ggplot(mpg, aes(x = displ, y = hwy, colour = class)) +
  ggrepel::geom_label_repel(
    aes(label = class),
    data = class_avg,
    size = 6,
    label.size = 0,
    segment.color = NA
  ) +
  geom_point() +
  theme(legend.position = "none")
```

این تکنیک برای افزودن یک برچسب منفرد در نمودار مفید است، اما نیاز به مقداری دقت دارد. اگر می‌خواهید متن را در گوشه‌های نمودار قرار دهید، من ترجیح می‌دهم که از `annotate()` استفاده کنم:

```{r}
label_info <- mpg %>%
  group_by(class) %>%
  summarise(
    displ = max(displ),
    hwy = max(hwy),
    .groups = "drop"
  )

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point() +
  annotate(
    geom = "label", x = Inf, y = Inf,
    label = "افزایش اندازه موتور ارتباط با\n کاهش کارایی سوخت دارد",
    hjust = "right", vjust = "top"
  )
```

اگر فقط می‌خواهید یک قسمت از نمودار را برجسته کنید، از بسته **gghighlight** استفاده کنید. در [مقطع قبلی] مثالی از استفاده از آن دیده‌اید.

### تمرینات

1. از `geom_text()` با `hjust` و `vjust` کار کنید تا دقیقاً متوجه شوید که چگونه آن‌ها کار می‌کنند.

2. چه اتفاقی می‌افتد اگر برچسب‌ها را به `geom_smooth()` اضافه کنید؟ چرا این مفید نیست؟

3. موقعیت برچسب در نمودار آخر را چگونه کنترل می‌کنید؟ (راهنمایی: در مورد استفاده از `hjust` و `vjust` فکر کنید.)

4. چرا ممکن است ترجیح دهید از `annotate()` به‌جای `geom_text()` استفاده کنید؟

## مقیاس‌ها

سومین راه برای بهتر کردن نمودارهایتان جهت ارتباط، تنظیم مقیاس‌ها است. مقیاس‌ها نحوه نقشه‌برداری داده‌ها از فضای داده به فضای زیبایی‌شناختی را کنترل می‌کنند. به‌طور عادی، ggplot2 این مقیاس‌ها را به‌طور خودکار برای شما اضافه می‌کند. به‌عنوان مثال، وقتی شما این کد را می‌نویسید:

```{r}
#| eval: false

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class))
```

ggplot2 این مقیاس‌ها را پشت صحنه به‌طور خودکار اضافه می‌کند:

```{r}
#| eval: false

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  scale_x_continuous() +
  scale_y_continuous() +
  scale_colour_discrete()
```

نام‌گذاری مقیاس‌ها با این الگو پیروی می‌کند: `scale_` و سپس نام زیبایی‌شناختی (`x`, `y`, یا `colour`)، سپس `_` و نام مقیاس (`continuous`, `discrete`, `datetime`, یا `date`). هنگامی که نیاز به تنظیم مقیاس پیش‌فرض دارید، باید یکی از این مقیاس‌ها را با دست اضافه کنید.

### توقف‌های محورها

استفاده‌های رایج‌تر برای `scale_x_*` و `scale_y_*` تنظیم توقف‌های محورها است. بیایید یک نمودار ساده بسازیم تا نحوه کار آن‌ها را ببینیم:

```{r}
base <- ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class))

base
```

```{r}
base + scale_x_continuous(breaks = seq(1, 7, by = 1))
```

```{r}
base + scale_x_continuous(
  breaks = seq(1, 7, by = 1), 
  labels = c("1.0", "2.0", "3.0", "4.0", "5.0", "6.0", "7.0")
)
```

توجه داشته باشید که `breaks` موقعیت‌هایی را تعریف می‌کند که توقف‌ها باید ظاهر شوند، و `labels` متن‌هایی که باید در هر توقف ظاهر شود. شما می‌توانید از `labels` برای نمایش متن دلخواه در هر توقف استفاده کنید.

یکی از استفاده‌های مفید `labels` اضافه کردن واحدها به محور‌ها است:

```{r}
base + scale_y_continuous(labels = scales::label_percent())
```

یا اضافه کردن پیشوندهایی مانند $ برای ارز:

```{r}
base + scale_y_continuous(labels = scales::label_dollar())
```

بسته **scales** تابع‌های زیادی برای برچسب‌گذاری معمول محورها فراهم می‌کند که از آن‌ها `label_percent()`, `label_dollar()`, `label_comma()` برخی مثال‌ها هستند.

### افسانه چیدمان

برای تنظیم کلی موقعیت افسانه، شما باید از تم theme استفاده کنید. ما به زودی در مورد تم‌ها صحبت خواهیم کرد، اما اکنون تنها آنچه باید بدانید این است که می‌توانید موقعیت افسانه را با `theme(legend.position = "...")` کنترل کنید:

```{r}
base <- ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class))

base + theme(legend.position = "left")
base + theme(legend.position = "top")
base + theme(legend.position = "bottom")
base + theme(legend.position = "right") # the default
```

همچنین می‌توانید از `theme(legend.position = "none")` استفاده کنید تا افسانه را کاملاً پنهان کنید.

برای کنترل نمایش افسانه‌های فردی، از `guides()` همراه با `guide_legend()` یا `guide_colourbar()` استفاده کنید. مثال زیر دو تکنیک مفید را نشان می‌دهد: کنترل تعداد ردیف‌هایی که افسانه استفاده می‌کند با `nrow`، و جایگزینی یکی از افسانه‌های پیش‌فرض با یک افسانه رنگ متداوم با `override.aes`:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_smooth(se = FALSE) +
  theme(legend.position = "bottom") +
  guides(colour = guide_legend(nrow = 1, override.aes = list(size = 4)))
```

### جایگزینی مقیاس

به‌جای تنظیم جزئیات یک مقیاس منفرد، می‌توانید کل مقیاس را جایگزین کنید. دو نوع مقیاس‌های رایج که ممکن است بخواهید جایگزین کنید، مقیاس‌های رنگ متداوم و رنگی هستند. خوشبختانه، همان اصول برای همه سایر زیبایی‌شناختی‌ها، بنابراین وقتی اصول رنگ را یاد گرفتید، آن‌ها را برای اندازه، شکل، خط‌کشی و غیره نیز خواهید توانست به کار ببرید.

به‌طور پیش‌فرض، ggplot2 از evenly spaced hues در فضای رنگ HSL استفاده می‌کند:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = drv))

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = drv)) +
  scale_colour_brewer(palette = "Set1")
```

هنگام اضافه کردن رنگ به نمودار، دو رنج انتخاب مهم وجود دارد که باید انجام دهید:

1. آیا رنگ‌ها باید مرتب (unordered) یا مرتب (ordered) باشند؟
2. آیا رنگ‌ها باید برای ویژگی‌های کیفی (qualitative) یا کمی (quantitative) استفاده شوند؟

علاوه بر این، می‌توانید از ColorBrewer استفاده کنید تا رنگ‌هایی انتخاب کنید که کار خود را به درستی انجام دهند. ColorBrewer ارائه می‌دهد سه نوع پالت‌های رنگی: sequential, diverging, و qualitative:

```{r}
#| fig-alt: >
#|   A diagram that shows three colour palettes: sequential, diverging, and 
#|   qualitative. Sequential scales can be either single-hue (blues) or 
#|   multi-hue (yellow-green-blue). Diverging scales are useful for data that 
#|   has a natural midpoint, or you want to highlight deviations from zero. 
#|   The qualitative scales provide a set of colours that are as different as 
#|   possible, making them perfect for displaying many different categories.

knitr::include_graphics("images/visualization-color-wheel.png")
```

شما می‌توانید به‌صورت دستی از ColorBrewer scales با `scale_colour_brewer()` استفاده کنید. در اینجا دو مثال آورده شده:

```{r}
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = drv)) +
  scale_colour_brewer(palette = "Set1")

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = drv, size = displ)) +
  scale_colour_brewer(palette = "Set1")
```

هنگامی که شما تعداد کمی از رنگ‌ها دارید، می‌توانید آن‌ها را به‌صورت دستی تنظیم کنید:

```{r}
presidential %>%
  mutate(id = 33 + row_number()) %>%
  ggplot(aes(x = start, y = id, colour = party)) +
  geom_point() +
  geom_segment(aes(xend = end, yend = id)) +
  scale_colour_manual(values = c(Republican = "red", Democratic = "blue"))
```

برای متغیرهای متداوم، می‌توانید از بسته **viridis** استفاده کنید که با داشتن شامل تابع‌هایی که مقیاس‌های رنگی perceptually uniform ارائه می‌دهند. ggplot2 `scale_colour_viridis_d()` برای مقیاس‌های گسسته و `scale_colour_viridis_c()` برای مقیاس‌های متداوم دارد.

```{r}
df <- tibble(
  x = rnorm(10000),
  y = rnorm(10000)
)

ggplot(df, aes(x, y)) +
  geom_hex() +
  coord_fixed()

ggplot(df, aes(x, y)) +
  geom_hex() +
  coord_fixed() +
  scale_fill_viridis_c()
```

توجه داشته باشید که همه مقیاس‌های رنگی به دو شکل می‌آیند: `scale_colour_x()` و `scale_fill_x()` برای `colour` و `fill` aesthetics به ترتیب.

### تمرینات

1. چرا کد زیر `legend.position = "bottom"` را نادیده می‌گیرد؟

```{r}
#| eval: false

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_smooth(se = FALSE) +
  theme(legend.position = "bottom") +
  guides(colour = guide_legend(
    nrow = 1,
    override.aes = list(size = 4, alpha = 1)
  ))
```

2. چه guides وجود دارند؟ از `?guide_legend` و `?guide_colourbar` شروع کنید.

3. چه اتفاقی می‌افتد اگر `override.aes` را از guide زیر حذف کنید؟ چرا فکر می‌کنید این کار مفید است؟

```{r}
#| eval: false

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = class)) +
  geom_smooth(se = FALSE) +
  theme(legend.position = "bottom") +
  guides(colour = guide_legend(
    nrow = 1,
    override.aes = list(size = 4)
  ))
```

4. کدی بنویسید که یک نمودار میله‌ای رنگی ایجاد کند که از `presidential` داده‌ها استفاده کند.

## تم‌ها

در نهایت، می‌توانید نمایش کلی نمودارتان را با تغییر تم سفارشی کنید. ggplot2 شامل [هشت تم][ggplot2-themes] است:

```{r}
#| fig-asp: 1
#| fig-width: 6
#| out-width: "50%"

base <- ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(color = class))

base + theme_bw()
base + theme_light()
base + theme_classic()
base + theme_linedraw()
base + theme_dark()
base + theme_minimal()
base + theme_gray()
base + theme_void()
```

تم پیش‌فرض `theme_gray()` است. اگر قصد انتشار نمودارهایتان را دارید، ممکن است بخواهید به `theme_minimal()` یا یکی از سایر تم‌ها تغییر دهید. بسیاری از نشریات تم‌های سفارشی خود دارند.

ممکن است تعجب کنید چرا تم پیش‌فرض زمینه خاکستری دارد. این به‌خاطر این است که grid lines را بی‌مزاحم می‌کند. grid lines مهم هستند چون به شما در نمودار navigation کمک می‌کنند و مقایسه موقعیت‌ها آسان می‌کنند، اما شما نمی‌خواهید آن‌ها توجه را از داده‌ها منحرف کنند. زمینه خاکستری grid lines سفید را با اولویت کمتر نشان می‌دهد در حالی که همچنان visible می‌مانند.

همچنین یک بسته **ggthemes** (https://jrnold.github.io/ggthemes) وجود دارد که مجموعه بیشتری از تم‌ها فراهم می‌کند.

برای کنترل دقیق‌تر روی تم، باید تابع `theme()` را یاد بگیرید. دو آرگومان اصلی برای `theme()` وجود دارد:

1. **element function**ها که چگونگی stylize کردن عناصر غیر داده‌ای نمودار را مشخص می‌کنند. `element_blank()`, `element_rect()`, `element_line()` و `element_text()` چهار element function اصلی هستند.

2. **element names** که کدام عنصر نمودار را stylize کنید مشخص می‌کند.

برای درک چگونگی کار آن‌ها، بیایید رنگ پانل را تغییر دهیم:

```{r}
base + theme(panel.background = element_rect(fill = "white", colour = "grey50"))
```

برای تغییر ظاهر متن در محورها:

```{r}
base + theme(axis.text = element_text(colour = "blue"))
```

برای درک کامل دستور `theme()`, پیشنهاد می‌کنم که دیاگرام را در https://henrywang.nl/ggplot2-theme-elements-reference/ مطالعه کنید.

### تمرینات

1. یک نمودار انتخابی ایجاد کنید و سپس آن را با یک تم متفاوت تغییر دهید. چه تفاوت‌هایی می‌بینید؟

2. چه theme settings باعث می‌شود که `theme_minimal()` حس minimal داشته باشد؟ آن را به `theme_gray()` مقایسه کنید.

## صرفه‌جویی در نمودارها

دو راه اصلی برای خروج از ggplot2 وجود دارد: `ggsave()` و knitr. `ggsave()` آخرین نمودارتان را در disk ذخیره می‌کند:

```{r}
#| eval: false

ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point()

ggsave("my-plot.pdf")
```

این کد آخرین نمودار را در فایل `my-plot.pdf` در working directory شما ذخیره می‌کند. اگر مشخص نکنید، `ggsave()` ابعاد فعلی device graphics را حدس می‌زند و از آن استفاده می‌کند، اما به‌طور کلی بهتر است ابعاد را به‌صراحت تنظیم کنید تا یک خروجی reproducible داشته باشید:

```{r}
#| eval: false

ggsave("my-plot.pdf", width = 6, height = 6)
```

بطور کلی، بهترین approach برای شامل کردن نمودارها در final reports، استفاده از knitr و رسانه‌ای مانند R Markdown است. شما این موضوع را در [R Markdown] یاد خواهید گرفت.

## خلاصه

در این فصل یاد گرفتید که چگونه ggplot2 را برای ارتباط با نمودارهای خود استفاده کنید. با یادگیری تکنیک‌های مختلف، از برچسب‌گذاری تا تم‌ها، اکنون می‌توانید نمودارهایی بسازید که نه تنها زیبا باشند، بلکه ایده‌هایتان را نیز به وضوح منتقل کنند.

نکات کلیدی این فصل:

1. استفاده از `labs()` برای اضافه کردن عنوان، زیرعنوان، caption و برچسب‌های محور
2. حاشیه‌نویسی نمودارها با `geom_text()`, `geom_label()` و بسته **ggrepel**
3. سفارشی کردن مقیاس‌ها برای بهبود نمایش داده‌ها
4. استفاده از تم‌های مختلف برای تغییر ظاهر کلی نمودارها
5. ذخیره نمودارها با `ggsave()`

در فصل بعدی، شروع به کار با ساختارهای داده‌ای مختلف خواهیم کرد.