# ۷ وارد کردن داده‌ها

## 1.7 مقدمه

کار با داده‌هایی که در خود R آماده شده‌اند، روش عالی برای یادگیری ابزارهای علم داده است، اما شما می‌خواهید آنچه را که آموخته‌اید در مقطعی روی داده‌های خود اعمال کنید. در این فصل، اصول اولیه خواندن فایل‌های داده در R را خواهید آموخت.

در این فصل، نحوه خواندن فایل‌های مستطیلی متن ساده به R را خواهید آموخت. ما با توصیه‌های عملی برای مدیریت ویژگی‌هایی مانند نام ستون‌ها، نوع داده‌ها و داده‌های گمشده شروع خواهیم کرد.  سپس در مورد خواندن داده‌ها از چندین فایل به طور همزمان و نوشتن داده‌ها از R به یک فایل یاد خواهید گرفت. در نهایت، یاد خواهید گرفت که چگونه چارچوب داده‌ها را در R به صورت دستی ایجاد کنید.

### 1.1.7 پیش‌نیازها

در این فصل، بارگذاری داده‌های جدولی در R را با استفاده از بسته **readr** یاد خواهید گرفت که بخشی از tidyverse اصلی است.

```{r}
library(tidyverse)
```

## 2.7 خواندن داده‌ها از یک فایل

برای شروع، بیایید بر روی رایج‌ترین شکل ذخیره‌سازی داده‌های مستطیلی یعنی csv متمرکز شویم. در اینجا یک فایل csv ساده را مشاهده می‌کنید. سطر اول، که معمولاً ردیف عنوان نامیده می‌شود، نام ستون‌ها را ارائه می‌دهد، و شش سطر بعدی داده‌ها را نشان می‌دهند. ستون‌ها با ویرگول از هم جدا شده‌اند.

```
Student ID,Full Name,favourite.food,mealPlan,AGE
1,Sunil Huffmann,Strawberry yoghurt,Lunch only,4
2,Barclay Lynn,French fries,Lunch only,5
3,Jayendra Lyne,N/A,Breakfast and lunch,7
4,Leon Rossini,Anchovies,Lunch only,
5,Chidiegwu Dunkel,Pizza,Breakfast and lunch,five
6,Güvenç Attila,Ice cream,Lunch only,6
```
جدول 1.7 نمایشی از همان داده‌ها را به صورت یک جدول نشان می‌دهد.

<div align="center">

**جدول 1.7: داده‌های فایل students.csv به صورت جدول.**
| Student ID | Full Name          | favourite.food       | mealPlan              | AGE   |
|------------|--------------------|----------------------|-----------------------|-------|
| 1          | Sunil Huffmann     | Strawberry yoghurt    | Lunch only            | 4     |
| 2          | Barclay Lynn       | French fries         | Lunch only            | 5     |
| 3          | Jayendra Lyne      | N/A                  | Breakfast and lunch    | 7     |
| 4          | Leon Rossini       | Anchovies            | Lunch only            |   NA   |
| 5          | Chidiegwu Dunkel   | Pizza                | Breakfast and lunch    | five  |
| 6          | Güvenç Attila      | Ice cream            | Lunch only            | 6     |
   
</div>



می‌توانید این فایل را با `()read_csv` بخوانید. اولین آرگومان مهم‌ترین آرگومان است: مسیر فایل. می‌توانید مسیر را به عنوان آدرس فایل در نظر بگیرید: نام فایل students.csv است و در پوشه data قرار دارد.


```{r}
students <- read_csv("data/students.csv")
#> Rows: 6 Columns: 5
#> ── Column specification ─────────────────────────────────────────────────────
#> Delimiter: ","
#> chr (4): Full Name, favourite.food, mealPlan, AGE
#> dbl (1): Student ID
#> 
#> ℹ Use `spec()` to retrieve the full column specification for this data.
#> ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

کد بالا در صورتی کار می‌کند که فایل students.csv را در پوشه‌ی data در پروژه‌ی خود داشته باشید. می‌توانید فایل students.csv را از آدرس [https://pos.it/r4ds-students-csv](https://pos.it/r4ds-students-csv)
 دانلود کنید یا می‌توانید آن را مستقیماً از آن URL با دستور زیر بخوانید:

```{r}
students <- read_csv("https://pos.it/r4ds-students-csv")
```


 
هنگامی که `()read_csv` را اجرا می‌کنید، پیامی چاپ می‌کند که تعداد ردیف‌ها و ستون‌های داده‌ها، جداکننده‌ای که استفاده شده است و مشخصات ستون (نام ستون‌ها که بر اساس نوع داده‌ای که ستون در آن قرار دارد سازماندهی شده‌اند) را به شما می‌گوید. همچنین اطلاعاتی در مورد بازیابی مشخصات کامل ستون و نحوه عدم نمایش این پیام چاپ می‌کند. این یک بخش مهم از readr است که بعداً در بخش 3.7 به آن برمی‌گردیم.


## 1.2.7 توصیه‌های کاربردی

وقتی داده‌ها را می‌خوانید، نخستین قدم معمولاً شامل تبدیل آن‌ها به نحوی است تا کار با آن‌ها در ادامه‌ی تحلیل آسان‌تر شود. بیایید با در نظر گرفتن این نکته، نگاهی دیگر به داده‌های  ‍‍ `students` بیندازیم.


```{r}
students
#> # A tibble: 6 × 5
#>   `Student ID` `Full Name`      favourite.food     mealPlan            AGE  
#>          <dbl> <chr>            <chr>              <chr>               <chr>
#> 1            1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 2            2 Barclay Lynn     French fries       Lunch only          5    
#> 3            3 Jayendra Lyne    N/A                Breakfast and lunch 7    
#> 4            4 Leon Rossini     Anchovies          Lunch only          <NA> 
#> 5            5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 6            6 Güvenç Attila    Ice cream          Lunch only          6
```

در ستون `favourite.food`، تعدادی غذا وجود دارد و سپس رشته کاراکتری `N/A` که باید یک `NA` واقعی می‌بود که R آن را به عنوان «موجود نیست» تشخیص می‌دهد. این چیزی است که می‌توانیم با استفاده از آرگومان ‍‍`na` به آن اشاره کنیم. به طور پیش‌فرض، `()read_csv` تنها رشته‌های خالی (`""`) را به عنوان مقادیر `NA` در نظر می‌گیرد، اما ما همچنین می‌خواهیم که رشته‌های کاراکتری `"N/A"` را نیز مقادیر گمشده تشخیص دهد.
```{r}
students <- read_csv("data/students.csv", na = c("N/A", ""))

students
#> # A tibble: 6 × 5
#>   `Student ID` `Full Name`      favourite.food     mealPlan            AGE  
#>          <dbl> <chr>            <chr>              <chr>               <chr>
#> 1            1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 2            2 Barclay Lynn     French fries       Lunch only          5    
#> 3            3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
#> 4            4 Leon Rossini     Anchovies          Lunch only          <NA> 
#> 5            5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 6            6 Güvenç Attila    Ice cream          Lunch only          6
```

همچنین ممکن است متوجه شوید که ستون‌های `Student ID` و `Full Name` با علامت بک‌تیک احاطه شده‌اند. دلیلش این است که آنها حاوی فاصله هستند و قوانین معمول R برای نام متغیرها را نقض می‌کنند؛ آنها نام‌های **غیرنحوی** هستند. برای اشاره به این متغیرها، باید آنها را درون علامت بک‌تیک قرار دهید، ‍‍`` ` ``:

```{r}
students |> 
  rename(
    student_id = `Student ID`,
    full_name = `Full Name`
  )
#> # A tibble: 6 × 5
#>   student_id full_name        favourite.food     mealPlan            AGE  
#>        <dbl> <chr>            <chr>              <chr>               <chr>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 2          2 Barclay Lynn     French fries       Lunch only          5    
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
#> 4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 6          6 Güvenç Attila    Ice cream          Lunch only          6
```

یک رویکرد جایگزین، استفاده از `()janitor::clean_names` برای استفاده از برخی روش‌های اکتشافی است تا همه آنها را به طور همزمان به حروف کوچک تبدیل کند.


```{r}
students |> janitor::clean_names()
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan           age  
#>        <dbl> <chr>            <chr>              <chr>               <chr>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 2          2 Barclay Lynn     French fries       Lunch only          5    
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
#> 4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 6          6 Güvenç Attila    Ice cream          Lunch only          6
```

یکی دیگر از کارهای رایج پس از خواندن داده‌ها، بررسی نوع متغیرها است. به عنوان مثال، `meal_plan` یک متغیر رسته‌ای با مجموعه‌ای از مقادیر ممکن شناخته‌شده است که در R باید به صورت یک عامل نمایش داده شود:

```{r}
students |>
  janitor::clean_names() |>
  mutate(meal_plan = factor(meal_plan))
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan           age  
#>        <dbl> <chr>            <chr>              <fct>               <chr>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 2          2 Barclay Lynn     French fries       Lunch only          5    
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch 7    
#> 4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 6          6 Güvenç Attila    Ice cream          Lunch only          6
```

توجه داشته باشید که مقادیر موجود در متغیر `meal_plan` ثابت مانده‌اند، اما نوع متغیری که در زیر نام متغیر نشان داده شده است از کاراکتر (`<chr>`) به فاکتور (`<fct>`) تغییر کرده است. در فصل ۱۶ درباره فاکتورها بیشتر خواهید آموخت.

قبل از اینکه این داده‌ها را تجزیه و تحلیل کنید، احتمالاً می‌خواهید ستون `age` را اصلاح کنید. در حال حاضر، سن یک متغیر از نوع کاراکتر است زیرا یکی از مشاهدات به جای عدد `5`، به صورت `five` چاپ شده است. جزئیات رفع این مشکل را در فصل ۲۰ بررسی خواهیم کرد.

```{r}
students <- students |>
  janitor::clean_names() |>
  mutate(
    meal_plan = factor(meal_plan),
    age = parse_number(if_else(age == "five", "5", age))
  )

students
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan             age
#>        <dbl> <chr>            <chr>              <fct>               <dbl>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
#> 2          2 Barclay Lynn     French fries       Lunch only              5
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch     7
#> 4          4 Leon Rossini     Anchovies          Lunch only             NA
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
#> 6          6 Güvenç Attila    Ice cream          Lunch only              6
```

یک تابع جدید در اینجا `()if_else` است که سه آرگومان دارد. آرگومان اول `test` باید یک بردار منطقی باشد. نتیجه شامل مقدار آرگومان دوم، `yes`، وقتی `test` برابر با `TRUE` باشد، و مقدار آرگومان سوم، `no`، وقتی `FALSE` باشد، خواهد بود. در اینجا می‌گوییم اگر `age` برابر با کاراکتر `"five"` باشد، آن را با `"5"`  جایگزین کند، در غیر این صورت مقدار پیش‌فرض `age` را تغییر ندهد. در فصل 12 درباره ‍‍`()if_else` و بردارهای منطقی بیشتر خواهید آموخت.

### 2.2.7 دیگر آرگومان‌ها

چند آرگومان مهم دیگر هم وجود دارد که باید به آنها اشاره کنیم و اگر ابتدا یک ترفند مفید را به شما نشان دهیم، اثبات آنها آسان‌تر خواهد بود: تابع `()read_csv` می‌تواند رشته‌های متنی را که شما ایجاد و مانند یک فایل CSV قالب‌بندی کرده‌اید، بخواند:
‍‍
```{r}
read_csv(
  "a,b,c
  1,2,3
  4,5,6"
)
#> # A tibble: 2 × 3
#>       a     b     c
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3
#> 2     4     5     6
```

معمولاً، تابع `()read_csv` از خط اول داده‌ها برای نام ستون‌ها استفاده می‌کند که یک قرارداد بسیار رایج است. اما همچنین غیرمعمول نیست که چند خط فراداده در بالای فایل گنجانده شود. می‌توانید از `skip = n` برای نادیده گرفتن ‍‍`n` خط نخست یا از ‍‍‍‍`"#" = comment` برای حذف تمام خطوطی که با (مثلاً) `#` شروع می‌شوند، استفاده کنید:

```{r}
read_csv(
  "The first line of metadata
  The second line of metadata
  x,y,z
  1,2,3",
  skip = 2
)
#> # A tibble: 1 × 3
#>       x     y     z
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3

read_csv(
  "# A comment I want to skip
  x,y,z
  1,2,3",
  comment = "#"
)
#> # A tibble: 1 × 3
#>       x     y     z
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3
```

در موارد دیگر، داده‌ها ممکن است نام ستون نداشته باشند. می‌توانید از `col_names = TRUE` استفاده کنید تا ‍‍`()read_csv` ردیف اول را به عنوان نام ستون‌ها در نظر نگیرد و در عوض آنها را به صورت متوالی از `X1` تا `Xn` نام‌گذاری کند:

```{r}
read_csv(
  "1,2,3
  4,5,6",
  col_names = FALSE
)
#> # A tibble: 2 × 3
#>      X1    X2    X3
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3
#> 2     4     5     6
```

یا می‌توانید یک بردار کاراکتری به آرگومان `col_names` بدهید که به عنوان نام ستون‌ها استفاده خواهد شد:
```{r}
read_csv(
  "1,2,3
  4,5,6",
  col_names = c("x", "y", "z")
)
#> # A tibble: 2 × 3
#>       x     y     z
#>   <dbl> <dbl> <dbl>
#> 1     1     2     3
#> 2     4     5     6
```

این آرگومان‌ها تمام چیزی هستند که برای خواندن اکثر فایل‌های CSV که در عمل با آنها مواجه خواهید شد، باید بدانید. (برای مابقی، باید فایل `csv.` خود را با دقت بررسی کنید و مستندات مربوط به آرگومان‌های فراوان دیگر تابع `()read_csv` را بخوانید.)

### 3.2.7 انواع دیگر فایل
وقتی که بر `()read_csv` مسلط شدید، استفاده از سایر توابع readr ساده است؛ فقط باید بدانید که از کدام تابع استفاده کنید:

- تابع `()read_csv2` فایل‌هایی را که با نقطه ویرگول از هم جدا شده‌اند، می‌خواند. این تابع‌ها به جای `,` از `;` برای جدا کردن فیلدها استفاده می‌کنند و در کشورهایی که از `,` به عنوان نشانگر اعشار استفاده می‌کنند، رایج هستند.
- تابع `()read_tsv` فایل‌هایی را که با تب از هم جدا شده‌اند، می‌خواند.
- تابع `()read_delim` فایل‌هایی را که دارای هر جداکننده‌ای باشند، می‌خواند و اگر جداکننده را مشخص نکرده باشید، سعی می‌کند به‌ طور خودکار آن را حدس بزند.
- تابع `()read_fwf` فایل‌های با عرض ثابت را می‌خواند. می‌توانید فیلدها را بر اساس عرض آنها با تابع `()fwf_widths` یا بر اساس موقعیت آنها با تابع `()fwf_positions` مشخص کنید.
- تابع `()read_table` نوع رایجی از فایل‌های با عرض ثابت را می‌خواند که در آن‌ها ستون‌ها با فاصله از هم جدا شده‌اند.
- تابع `()read_log` فایل‌های log به سبک آپاچی را می‌خواند.


### 4.2.7 تمرین‌ها
1. برای خواندن فایلی که فیلدهای آن با «|» از هم جدا شده‌اند، از چه تابعی استفاده می‌کنید؟
2. جدا از `file`، `skip` و `comment`، چه آرگومان‌های دیگری بین `()read_csv` و `()read_tsv` مشترک است؟
3. مهمترین آرگومان‌های تابع `()read_fwf` چیست؟
4. گاهی اوقات رشته‌ها در یک فایل CSV حاوی ویرگول هستند. برای جلوگیری از ایجاد مشکل، باید آنها را با یک کاراکتر نقل قول مانند `"` یا `'` احاطه کرد. به طور پیش‌فرض، `()read_csv` فرض می‌کند که کاراکتر نقل قول `"` خواهد بود. برای خواندن متن زیر در یک چارچوب داده، چه آرگومانی را باید برای `()read_csv` مشخص کنید؟

```{r}
"x,y\n1,'a,b'"
```


5. مشکل هر یک از فایل‌های CSV درون‌خطی زیر را مشخص کنید. هنگام اجرای کد چه اتفاقی می‌افتد؟

```{r}
read_csv("a,b\n1,2,3\n4,5,6")
read_csv("a,b,c\n1,2\n1,2,3,4")
read_csv("a,b\n\"1")
read_csv("a,b\n1,2\na,b")
read_csv("a;b\n1;3")
```

6. با استفاده از روش‌های زیر، ارجاع به نام‌های غیر نحوی را در چارچوب داده زیر تمرین کنید:
   - استخراج متغیری که `1` نامیده می‌شود.
   - نمودار پراکنش `1` را در برابر `2` رسم کنید.
   - ایجاد یک ستون جدید به نام `3` که حاصل تقسیم `2` بر `1` است.
   - نام ستون‌ها را به `one`، `two` و `three` تغییر دهید.

```{r}
annoying <- tibble(
  `1` = 1:10,
  `2` = `1` * 2 + rnorm(length(`1`))
)
```

## 3.7 کنترل نوع داده ستون‌ها

یک فایل CSV هیچ اطلاعاتی در مورد نوع هر متغیر (مثلاً اینکه آیا منطقی، عددی، رشته‌ای و غیره است) ندارد، بنابراین readr سعی می‌کند نوع آن را حدس بزند. این بخش نحوه عملکرد فرآیند حدس زدن، نحوه حل برخی از مشکلات رایج که باعث ایجاد خطا می‌شوند و در صورت نیاز، نحوه ارائه نوع ستون‌ها توسط خودتان را شرح می‌دهد.  در نهایت، به چند راهبرد کلی اشاره خواهیم کرد که در صورت خرابی فاجعه‌بار readr و نیاز به کسب اطلاعات بیشتر در مورد ساختار فایل خود، مفید هستند.

### 1.3.7 حدس نوع داده

بسته readr از یک روش اکتشافی برای تشخیص نوع ستون‌ها استفاده می‌کند. برای هر ستون، مقادیر ۱۰۰۰ ردیف را که به صورت زوجی از ردیف اول تا آخر فاصله دارند، بدون در نظر گرفتن مقادیر گمشده، بیرون می‌کشد. سپس به سوالات زیر پاسخ می‌دهد:
- آیا فقط شامل `F`، `T`، `FALSE` یا `TRUE` (صرف نظر از حروف بزرگ و کوچک) است؟ اگر چنین است، منطقی است.
- آیا فقط شامل اعداد است (مثلاً `1`، `4.5-`، `5e6`، `Inf`)؟ اگر چنین است، یک عدد است.
- آیا با استاندارد ISO8601 مطابقت دارد؟ اگر چنین است، یک تاریخ یا تاریخ-زمان است. (در بخش 2.17 با جزئیات بیشتر به تاریخ-زمان‌ها باز خواهیم گشت).
- در غیر این صورت، باید یک رشته باشد.

می‌توانید نحوه انجام این کار را در عمل در مثال ساده زیر مشاهده کنید:

```{r}
read_csv("
  logical,numeric,date,string
  TRUE,1,2021-01-15,abc
  false,4.5,2021-02-15,def
  T,Inf,2021-02-16,ghi
")
#> # A tibble: 3 × 4
#>   logical numeric date       string
#>   <lgl>     <dbl> <date>     <chr> 
#> 1 TRUE        1   2021-01-15 abc   
#> 2 FALSE       4.5 2021-02-15 def   
#> 3 TRUE      Inf   2021-02-16 ghi
```

این روش اکتشافی اگر مجموعه داده‌ی تمیزی داشته باشید خوب کار می‌کند، اما در زندگی واقعی، با مجموعه‌ای از شکست‌های عجیب و زیبا روبرو خواهید شد.

### 2.3.7 مقادیر گمشده، نوع ستون‌ها و مشکلات

رایج‌ترین شیوه‌ای که تشخیص نوع ستون با شکست مواجه می‌شود این است که یک ستون حاوی مقادیر غیرمنتظره باشد و شما به جای یک نوع داده خاص، یک ستون کاراکتری دریافت می‌کنید. یکی از رایج‌ترین دلایل این امر، مقدار گمشده است که با استفاده از چیزی غیر از `NA`، مورد انتظار readr ثبت شده است. 

به عنوان مثال، فایل CSV ساده با یک ستون را در نظر بگیرید:

```{r}
simple_csv <- "
  x
  10
  .
  20
  30"
```

اگر آن را بدون هیچ آرگومان اضافی بخوانیم، `x` به یک ستون کاراکتری تبدیل می‌شود:
```{r}
read_csv(simple_csv)
#> # A tibble: 4 × 1
#>   x    
#>   <chr>
#> 1 10   
#> 2 .    
#> 3 20   
#> 4 30
```

در این مورد مثال بسیار ساده، به راحتی می‌توانید مقدار گمشده ‍‍`.` را ببینید. اما اگر هزاران ردیف داشته باشید که تنها چند مقدار گمشده با `.` در بین آنها پراکنده شده باشد، چه اتفاقی می‌افتد؟  یک رویکرد این است که در readr تعیین کنید x یک ستون عددی است و سپس ببینید کجا ایجاد خطا می‌کند. می‌توانید این کار را با آرگومان ‍‍`col_types` انجام دهید، که یک فهرست نامگذاری‌شده را می‌گیرد که در آن نام‌ها با نام ستون‌ها در فایل CSV مطابقت دارند:
```{r}
df <- read_csv(
  simple_csv, 
  col_types = list(x = col_double())
)
#> Warning: One or more parsing issues, call `problems()` on your data frame for
#> details, e.g.:
#>   dat <- vroom(...)
#>   problems(dat)
```

حالا تابع `()read_csv` گزارش می‌دهد که مشکلی وجود دارد و به ما می‌گوید که می‌توانیم با استفاده از `()problems` اطلاعات بیشتری کسب کنیم:
```{r}
problems(df)
#> # A tibble: 1 × 5
#>     row   col expected actual file                            
#>   <int> <int> <chr>    <chr>  <chr>                           
#> 1     3     1 a double .      /tmp/RtmpbZGAVt/file1fe02ab47d03
```

این به ما می‌گوید که در ردیف ۳، ستون ۱، مشکلی وجود داشته است که در آن readr انتظار یک عدد را داشته اما مقدار `.` دریافت کرده است. این نشان می‌دهد که این مجموعه داده از `.` برای مقادیر گمشده استفاده می‌کند. بنابراین، در تابع ‍‍`"." = na` قرار می‌دهیم، حدس خودکار موفقیت‌آمیز است و ستون عددی مورد نظر را به ما می‌دهد:
```{r}
read_csv(simple_csv, na = ".")
#> # A tibble: 4 × 1
#>       x
#>   <dbl>
#> 1    10
#> 2    NA
#> 3    20
#> 4    30
```

### 3.3.7 نوع داده ستون‌ها

بسته readr در مجموع نه نوع ستون برای استفاده شما فراهم می‌کند:
- توابع `()col_logical` و `()col_double` اعداد منطقی و حقیقی را می‌خوانند. آنها نسبتاً به ندرت مورد نیاز هستند (به جز موارد فوق)، زیرا readr معمولاً آنها را برای شما حدس می‌زند.
- تابع `()col_integer` اعداد صحیح را می‌خواند. ما در این کتاب به ندرت بین اعداد صحیح و اعداد اعشاری تمایز قائل می‌شویم زیرا از نظر عملکردی معادل هستند، اما خواندن صریح اعداد صحیح گاهی اوقات می‌تواند مفید باشد زیرا در مقایسه با اعداد اعشاری نیمی از حافظه را اشغال می‌کنند.
- تابع `()col_character` رشته‌ها را می‌خواند. این تابع می‌تواند برای مشخص کردن صریح ستونی که شناسه عددی دارد، مفید باشد. مثلاً مجموعه‌ای طولانی از ارقام که یک شیء را شناسایی می‌کند اما اعمال عملیات ریاضی روی آن منطقی نیست. مثال‌ها شامل شماره تلفن، شماره تأمین اجتماعی، شماره کارت اعتباری و غیره هستند.
- توابع `()col_factor` ،`()col_date` و `()col_datetime` به ترتیب عامل‌ها، تاریخ‌ها و تاریخ-زمان‌ها را ایجاد می‌کنند؛ وقتی در فصل‌های 16 و 17 به این نوع داده‌ها بپردازیم، اطلاعات بیشتری در مورد آنها کسب خواهید کرد.
- تابع `()col_number` یک تجزیه‌گر عددی مجاز است که اجزای غیرعددی را نادیده می‌گیرد و به ویژه برای واحد پول‌ها مفید است. در فصل ۱۳ درباره آن بیشتر خواهید آموخت.
- تابع `()col_skip` یک ستون را در نظر نمی‌گیرد، بنابراین در نتیجه لحاظ نمی‌شود، که می‌تواند برای افزایش سرعت خواندن داده‌ها در صورتی که فایل CSV بزرگی دارید و فقط می‌خواهید از برخی از ستون‌ها استفاده کنید، مفید باشد.

همچنین، می‌توان با تغییر از `()list` به `()cols` و مشخص کردن مقدار `default.` در آن، حدس روش اکتشافی تابع را لغو کرد.
```{r}
another_csv <- "
x,y,z
1,2,3"

read_csv(
  another_csv, 
  col_types = cols(.default = col_character())
)
#> # A tibble: 1 × 3
#>   x     y     z    
#>   <chr> <chr> <chr>
#> 1 1     2     3
```

یکی دیگر از توابع کمکی مفید، تابع `()cols_only` است که فقط ستون‌هایی را که شما مشخص می‌کنید، می‌خواند:
```{r}
read_csv(
  another_csv,
  col_types = cols_only(x = col_character())
)
#> # A tibble: 1 × 1
#>   x    
#>   <chr>
#> 1 1
```

## 4.7 خواندن داده‌ها از چندین فایل

گاهی اوقات داده‌های شما به جای اینکه در یک فایل واحد قرار گیرند، در چندین فایل تقسیم می‌شوند. برای مثال، ممکن است داده‌های فروش چندین ماه را داشته باشید و داده‌های هر ماه در یک فایل جداگانه قرار داشته باشند: `01-sales.csv` برای ژانویه، `02-sales.csv` برای فوریه و `03-sales.csv` برای مارس. با `()read_csv` می‌توانید این داده‌ها را به طور همزمان بخوانید و آنها را در یک چارچوب داده واحد روی هم قرار دهید.

```{r}
sales_files <- c("data/01-sales.csv", "data/02-sales.csv", "data/03-sales.csv")
read_csv(sales_files, id = "file")
#> # A tibble: 19 × 6
#>   file              month    year brand  item     n
#>   <chr>             <chr>   <dbl> <dbl> <dbl> <dbl>
#> 1 data/01-sales.csv January  2019     1  1234     3
#> 2 data/01-sales.csv January  2019     1  8721     9
#> 3 data/01-sales.csv January  2019     1  1822     2
#> 4 data/01-sales.csv January  2019     2  3333     1
#> 5 data/01-sales.csv January  2019     2  2156     9
#> 6 data/01-sales.csv January  2019     2  3987     6
#> # ℹ 13 more rows
```

یک بار دیگر، کد بالا در صورتی کار خواهد کرد که فایل‌های CSV را در پوشه داده در پروژه خود داشته باشید.
می‌توانید این فایل‌ها را از [https://pos.it/r4ds-01-sales](https://pos.it/r4ds-01-sales)، [https://pos.it/r4ds-02-sales](https://pos.it/r4ds-02-sales) و [https://pos.it/r4ds-03-sales](https://pos.it/r4ds-03-sales) دانلود کنید یا می‌توانید آنها را مستقیماً با دستور زیر بخوانید:

```{r}
sales_files <- c(
  "https://pos.it/r4ds-01-sales",
  "https://pos.it/r4ds-02-sales",
  "https://pos.it/r4ds-03-sales"
)
read_csv(sales_files, id = "file")
```

آرگومان `id` یک ستون جدید به نام `file` به چارچوب داده حاصل اضافه می‌کند و فایلی که داده‌ها از آن می‌آیند را مشخص می‌کند. این امر به ویژه در شرایطی مفید است که فایل‌هایی که در حال خواندن آنها هستید، ستون منحصر به فردی ندارند که بتواند به شما کمک کنند که داده‌ها مربوط به کدام یک از فایل‌ها هستند.

اگر فایل‌های زیادی دارید که می‌خواهید داده‌هایشان را بخوانید، نوشتن نام آنها به صورت یک فهرست می‌تواند دردسرساز شود. در عوض، می‌توانید از تابع پایه `()list.files` برای یافتن فایل‌ها با تطبیق یک الگو در نام فایل‌ها استفاده کنید. در فصل 15 درباره این الگوها بیشتر خواهید آموخت.

```{r}
sales_files <- list.files("data", pattern = "sales\\.csv$", full.names = TRUE)
sales_files
#> [1] "data/01-sales.csv" "data/02-sales.csv" "data/03-sales.csv"
```


## 5.7 ذخیره‌سازی داده‌ها

همچنین بسته readr دو تابع مفید برای ذخیره‌سازی داده‌ها دارد: `()write_csv` و `()write_tsv`. مهم‌ترین آرگومان‌های این توابع `x` (چارچوب داده‌ای که باید ذخیره شود) و `file` (محل ذخیره آن) هستند. همچنین می‌توانید با ‍‍`na` مشخص کنید که مقادیر گمشده چگونه ذخیره شوند و اگر بخواهید داده‌ها به یک فایل موجود اضافه شوند از `append` استفاده می‌کنید.
```{r}
write_csv(students, "students.csv")
```

حالا بیایید این فایل csv را دوباره بخوانیم. توجه داشته باشید که اطلاعات نوع متغیری که تازه تنظیم کرده‌اید، هنگام ذخیره در CSV از بین می‌رود، زیرا دوباره با خواندن از یک فایل متنی ساده شروع می‌کنید:

```{r}
students
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan             age
#>        <dbl> <chr>            <chr>              <fct>               <dbl>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
#> 2          2 Barclay Lynn     French fries       Lunch only              5
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch     7
#> 4          4 Leon Rossini     Anchovies          Lunch only             NA
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
#> 6          6 Güvenç Attila    Ice cream          Lunch only              6
write_csv(students, "students-2.csv")
read_csv("students-2.csv")
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan             age
#>        <dbl> <chr>            <chr>              <chr>               <dbl>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
#> 2          2 Barclay Lynn     French fries       Lunch only              5
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch     7
#> 4          4 Leon Rossini     Anchovies          Lunch only             NA
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
#> 6          6 Güvenç Attila    Ice cream          Lunch only              6
```

این باعث می‌شود CSVها برای ذخیره نتایج موقت کمی غیرقابل اعتماد باشند - شما هر بار که داده‌ها را می‌خوانید باید مشخصات ستون را از نو ایجاد کنید. دو جایگزین اصلی وجود دارد:

1. توابع `()write_rds` و `()read_rds` معادل‌های یکسانی همانند توابع پایه `()readRDS` و `()saveRDS` هستند. این توابع داده‌ها را در قالب دودویی مختص R به نام RDS ذخیره می‌کنند. این بدان معناست که وقتی داده‌ها را دوباره می‌خوانید، دقیقاً همان نوع داده R را که ذخیره کرده‌اید بارگذاری می‌کنید.

```{r}
write_rds(students, "students.rds")
read_rds("students.rds")
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan             age
#>        <dbl> <chr>            <chr>              <fct>               <dbl>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
#> 2          2 Barclay Lynn     French fries       Lunch only              5
#> 3          3 Jayendra Lyne    <NA>               Breakfast and lunch     7
#> 4          4 Leon Rossini     Anchovies          Lunch only             NA
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
#> 6          6 Güvenç Attila    Ice cream          Lunch only              6
```

2. بسته‌ی arrow به شما امکان خواندن و نوشتن فایل‌های parquet را می‌دهد، یک فرمت فایل باینری سریع که می‌تواند بین زبان‌های برنامه‌نویسی به اشتراک گذاشته شود. در فصل ۲۲ به طور مفصل‌تر به arrow خواهیم پرداخت.

```{r}
library(arrow)
write_parquet(students, "students.parquet")
read_parquet("students.parquet")
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan             age
#>        <dbl> <chr>            <chr>              <fct>               <dbl>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only              4
#> 2          2 Barclay Lynn     French fries       Lunch only              5
#> 3          3 Jayendra Lyne    NA                 Breakfast and lunch     7
#> 4          4 Leon Rossini     Anchovies          Lunch only             NA
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch     5
#> 6          6 Güvenç Attila    Ice cream          Lunch only              6
```

قابل داده parquet معمولاً بسیار سریع‌تر از RDS است و خارج از R نیز قابل استفاده است، اما به بسته arrow نیاز دارد.


## 6.7 ورود داده

گاهی اوقات لازم است که یک جدول را «به صورت دستی» و با وارد کردن کمی داده در اسکریپت R خود، آماده کنید. دو تابع مفید برای کمک به شما در انجام این کار وجود دارد که در چیدمان جدول بر اساس ستون یا ردیف متفاوت هستند. ‍‍`()tibble` بر اساس ستون کار می‌کند:

```{r}
tibble(
  x = c(1, 2, 5), 
  y = c("h", "m", "g"),
  z = c(0.08, 0.83, 0.60)
)
#> # A tibble: 3 × 3
#>       x y         z
#>   <dbl> <chr> <dbl>
#> 1     1 h      0.08
#> 2     2 m      0.83
#> 3     5 g      0.6
```

چیدمان داده‌ها بر اساس ستون می‌تواند مشاهده ارتباط ردیف‌ها را دشوار کند، بنابراین یک جایگزین `()tribble` است که مخفف **tr**ansposed **ti**bble است و به شما امکان می‌دهد داده‌های خود را ردیف به ردیف چیدمان کنید. `()tribble` برای ورود داده‌ها در کد طراحی شده است: عنوان ستون‌ها با ‍‍‍‍`~` شروع می‌شوند و ورودی‌ها با ویرگول از هم جدا می‌شوند. این امر امکان چیدمان مقادیر کم داده‌ها را به شکلی آسان برای خواندن فراهم می‌کند:

```{r}
tribble(
  ~x, ~y, ~z,
  1, "h", 0.08,
  2, "m", 0.83,
  5, "g", 0.60
)
#> # A tibble: 3 × 3
#>       x y         z
#>   <dbl> <chr> <dbl>
#> 1     1 h      0.08
#> 2     2 m      0.83
#> 3     5 g      0.6
```

## 7.7 خلاصه

در این فصل، یاد گرفتید که چگونه فایل‌های CSV را با `()read_csv` بارگذاری کنید و با `()tibble` و `()tribble` داده‌های خود را وارد کنید. یاد گرفتید که فایل‌های csv چگونه کار می‌کنند، برخی از مشکلاتی که ممکن است با آنها مواجه شوید و نحوه غلبه بر آنها را یاد گرفتید. در این کتاب چند بار به وارد کردن داده‌ها خواهیم پرداخت: فصل 20 از اکسل و گوگل شیت، فصل 21 نحوه بارگذاری داده‌ها از پایگاه‌های داده، فصل 22 از فایل‌های parquet، فصل 23 از JSON و فصل 24 از وب‌سایت‌ها را به شما نشان خواهیم داد.

تقریباً به پایان این بخش از کتاب رسیده‌ایم، اما یک موضوع مهم دیگر باقی مانده است که باید به آن بپردازیم: چگونه از راهنمای R استفاده کنیم. بنابراین در فصل بعد، شما با برخی از مکان‌های خوب برای جستجوی راهنما، نحوه ایجاد یک reprex برای به حداکثر رساندن شانس خود برای دریافت کمک خوب و برخی توصیه‌های کلی در مورد همگام شدن با دنیای R آشنا خواهید شد.
