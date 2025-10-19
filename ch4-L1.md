# ۲۰ صفحات گسترده

## ۱.۲۰ مقدمه

در فصل ۷ نحوه وارد کردن داده‌ها از فایل‌های متنی ساده مانند `.csv` و `.tsv` را یاد گرفتید. اکنون زمان آن است که یاد بگیرید چگونه داده‌ها را از یک صفحه گسترده، خواه یک صفحه گسترده اکسل یا یک Google Sheet، دریافت کنید. این مطلب بر اساس بسیاری از آنچه در فصل ۷ آموختید بنا شده است، اما ما همچنین در مورد ملاحظات و پیچیدگی‌های اضافی هنگام کار با داده‌های صفحات گسترده بحث خواهیم کرد.

اگر شما یا همکارانتان از صفحات گسترده برای سازماندهی داده‌ها استفاده می‌کنید، به شدت توصیه می‌کنیم مقاله "Data Organization in Spreadsheets" نوشته Karl Broman و Kara Woo را مطالعه کنید: https://doi.org/10.1080/00031305.2017.1375989. بهترین شیوه‌های ارائه شده در این مقاله زمانی که داده‌ها را از یک صفحه گسترده به R وارد می‌کنید تا تجزیه و تحلیل و تجسم کنید، شما را از سردردهای زیادی نجات می‌دهد.

## ۲.۲۰ اکسل

Microsoft Excel یک نرم‌افزار صفحه گسترده پرکاربرد است که در آن داده‌ها در کاربرگ‌ها درون فایل‌های صفحه گسترده سازماندهی می‌شوند.

### ۱.۲.۲۰ پیش‌نیازها

در این بخش، یاد خواهید گرفت که چگونه داده‌ها را از صفحات گسترده اکسل در R با بسته **readxl** بارگذاری کنید. این بسته جزو هسته tidyverse نیست، بنابراین باید آن را به صورت صریح بارگذاری کنید، اما هنگامی که بسته tidyverse را نصب می‌کنید، به طور خودکار نصب می‌شود. بعداً، از بسته writexl نیز استفاده خواهیم کرد که به ما امکان می‌دهد صفحات گسترده اکسل ایجاد کنیم.

```{r}
library(readxl)
library(tidyverse)
library(writexl)
```

### ۲.۲.۲۰ شروع کار

بیشتر توابع readxl به شما اجازه می‌دهند صفحات گسترده اکسل را در R بارگذاری کنید:

- `read_xls()` فایل‌های اکسل با فرمت `xls` را می‌خواند.
- `read_xlsx()` فایل‌های اکسل با فرمت `xlsx` را می‌خواند.
- `read_excel()` می‌تواند فایل‌ها با هر دو فرمت `xls` و `xlsx` را بخواند. نوع فایل را بر اساس ورودی حدس می‌زند.

این توابع همگی دارای نحو مشابهی هستند درست مانند توابع دیگری که قبلاً برای خواندن انواع دیگر فایل‌ها معرفی کرده‌ایم، مثلاً `read_csv()`، `read_table()` و غیره. برای بقیه این فصل روی استفاده از `read_excel()` تمرکز خواهیم کرد.

### ۳.۲.۲۰ خواندن صفحات گسترده اکسل

شکل ۲۰.۱ نشان می‌دهد که صفحه گسترده‌ای که قرار است در R بخوانیم در اکسل چگونه به نظر می‌رسد. این صفحه گسترده را می‌توان به عنوان یک فایل اکسل از https://docs.google.com/spreadsheets/d/1V1nPp1tzOuutXFLb3G9Eyxi3qxeEhnOXUzL5_BcCQ0w/ دانلود کرد.

اولین آرگومان `read_excel()` مسیر فایلی است که باید خوانده شود.

```{r}
students <- read_excel("data/students.xlsx")
```

`read_excel()` فایل را به عنوان یک tibble می‌خواند.

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

ما شش دانش‌آموز در داده‌ها و پنج متغیر برای هر دانش‌آموز داریم. با این حال چند چیز وجود دارد که ممکن است بخواهیم در این مجموعه داده به آن‌ها رسیدگی کنیم:

- نام ستون‌ها به هم ریخته هستند. می‌توانید نام ستون‌هایی را که از یک فرمت ثابت پیروی می‌کنند ارائه دهید؛ ما `snake_case` را با استفاده از آرگومان `col_names` توصیه می‌کنیم.

```{r}
read_excel(
  "data/students.xlsx",
  col_names = c("student_id", "full_name", "favourite_food", "meal_plan", "age")
)
#> # A tibble: 7 × 5
#>   student_id full_name        favourite_food     meal_plan           age  
#>   <chr>      <chr>            <chr>              <chr>               <chr>
#> 1 Student ID Full Name        favourite.food     mealPlan            AGE  
#> 2 1          Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 3 2          Barclay Lynn     French fries       Lunch only          5    
#> 4 3          Jayendra Lyne    N/A                Breakfast and lunch 7    
#> 5 4          Leon Rossini     Anchovies          Lunch only          <NA> 
#> 6 5          Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 7 6          Güvenç Attila    Ice cream          Lunch only          6
```

متأسفانه، این کاملاً کار را انجام نداد. اکنون نام متغیرهایی که می‌خواهیم را داریم، اما آنچه قبلاً سطر سرتیتر بود اکنون به عنوان اولین مشاهده در داده‌ها ظاهر می‌شود. می‌توانید به صراحت آن سطر را با استفاده از آرگومان `skip` رد کنید.

```{r}
read_excel(
  "data/students.xlsx",
  col_names = c("student_id", "full_name", "favourite_food", "meal_plan", "age"),
  skip = 1
)
#> # A tibble: 6 × 5
#>   student_id full_name        favourite_food     meal_plan           age  
#>        <dbl> <chr>            <chr>              <chr>               <chr>
#> 1          1 Sunil Huffmann   Strawberry yoghurt Lunch only          4    
#> 2          2 Barclay Lynn     French fries       Lunch only          5    
#> 3          3 Jayendra Lyne    N/A                Breakfast and lunch 7    
#> 4          4 Leon Rossini     Anchovies          Lunch only          <NA> 
#> 5          5 Chidiegwu Dunkel Pizza              Breakfast and lunch five 
#> 6          6 Güvenç Attila    Ice cream          Lunch only          6
```

- در ستون `favourite_food`، یکی از مشاهدات `N/A` است که باید به عنوان `NA` واقعی R ثبت شود.

```{r}
read_excel(
  "data/students.xlsx",
  col_names = c("student_id", "full_name", "favourite_food", "meal_plan", "age"),
  skip = 1,
  na = c("", "N/A")
)
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

- یک مشاهده دیگر در ستون `age` هست که متنی است، نه عددی (`"five"` به جای `5`). متأسفانه این بدان معنی است که `age` به عنوان یک ستون کاراکتری خوانده می‌شود. بعداً در این فصل در مورد انواع داده‌های ستون بحث خواهیم کرد، اما در حال حاضر می‌توانیم آن را برای ثابت کردن اینکه `age` در واقع یک ستون عددی است با استفاده از آرگومان `col_types` برطرف کنیم.

```{r}
students <- read_excel(
  "data/students.xlsx",
  col_names = c("student_id", "full_name", "favourite_food", "meal_plan", "age"),
  skip = 1,
  na = c("", "N/A"),
  col_types = c("numeric", "text", "text", "text", "text")
)

students
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

با این حال، این همچنان مشکل را به طور کامل حل نمی‌کند زیرا `age` هنوز هم یک ستون کاراکتری است. آنچه اتفاق افتاده این است که `col_types` از انواع داده‌ای که شما می‌توانید تعریف کنید محدود است و "numeric" از نظر اکسل یک عدد با نقاط اعشاری است. به همین دلیل آن‌ها را "double" می‌خوانیم. و `"five"` یک عدد اعشاری نیست، بنابراین وقتی می‌خواهیم این ستون را به عنوان یک عدد اعشاری بخوانیم با خطا مواجه نمی‌شویم، اما به عوض آن به عنوان یک ستون کاراکتری خوانده می‌شود. بنابراین، باید این مقدار را به `NA` تبدیل کنید و سپس `age` را به عدد تبدیل کنید. همانند مثال ساختگی ما، اگر داده‌های شما به مشکلات مشابهی دچار باشد، ممکن است نیاز داشته باشید داده‌های خوانده شده را با توابعی که در بخش‌های بعدی کتاب ارائه می‌شود پاکسازی کنید.

```{r}
students <- students |>
  mutate(
    age = if_else(age == "five", "5", age),
    age = parse_number(age)
  )

students
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

هیچ‌گاه نباید از دست دادن اطلاعات را نادیده بگیرید یا داده‌ها را به سادگی تغییر دهید و آن را در جای خود قرار دهید. اگر این داده‌ها به اندازه کافی مهم هستند که در اکسل جمع‌آوری شوند و سپس در R تجزیه و تحلیل شوند، احتمالاً به اندازه کافی مهم هستند که روند پاکسازی داده‌ها را مستند کنید. شما باید یک اسکریپت پاکسازی داده ایجاد کنید که مشکلات داده‌های خام را برطرف کند و همیشه می‌توانید داده‌های خام خود را نگه دارید تا در صورت کشف یک مشکل در فرآیند پاکسازی داده‌ها، بتوانید به آن‌ها بازگردید.

### ۴.۲.۲۰ خواندن کاربرگ‌ها

یک پارامتر مهم دیگر که می‌توانید در `read_excel()` استفاده کنید `sheet` است که به شما امکان می‌دهد کدام کاربرگ را می‌خواهید بخوانید مشخص کنید. پیش‌فرض این است که اولین کاربرگ خوانده می‌شود.

```{r}
read_excel("data/penguins.xlsx", sheet = "Torgersen Island")
#> # A tibble: 52 × 8
#>   species island    bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
#>   <chr>   <chr>              <dbl>         <dbl>             <dbl>       <dbl>
#> 1 Adelie  Torgersen           39.1          18.7               181        3750
#> 2 Adelie  Torgersen           39.5          17.4               186        3800
#> 3 Adelie  Torgersen           40.3          18                 195        3250
#> 4 Adelie  Torgersen           NA            NA                  NA          NA
#> 5 Adelie  Torgersen           36.7          19.3               193        3450
#> 6 Adelie  Torgersen           39.3          20.6               190        3650
#> # ℹ 46 more rows
#> # ℹ 2 more variables: sex <chr>, year <dbl>
```

برخی نفرات دوست دارند با صفحات گسترده کار کنند که شامل اطلاعات فراداده در بالای صفحه است - برای مثال شامل عنوان، توضیحات یا تاریخ‌ها. اگر می‌خواهید این اطلاعات را در R بخوانید، می‌توانید از `excel_sheets()` برای بدست آوردن اطلاعات درباره تمام کاربرگ‌های یک فایل اکسل استفاده کنید، و سپس از `read_excel()` برای خواندن کاربرگ‌هایی که علاقه‌مند هستید استفاده کنید.

```{r}
excel_sheets("data/penguins.xlsx")
#> [1] "Torgersen Island" "Biscoe Island"    "Dream Island"
```

هنگامی که کار با صفحات گسترده می‌کنید، اطلاعاتی که می‌خواهید بخوانید ممکن است در کاربرگ نباشد، بلکه در محدوده سلول‌های کاربرگ باشد. یک رویکرد کلاسیک برای ذخیره داده‌های جدولی در یک صفحه گسترده به طوری که از قابلیت‌های قالب‌بندی سلول استفاده کند، قرار دادن این جدول در یک محدوده است که شامل سایر سلول‌های خالی پیرامون آن است. شکل ۲۰.۲ نشان می‌دهد این چه شکلی به نظر می‌رسد.

در این مورد، شما می‌توانید آرگومان `range` را به `read_excel()` بدهید تا مشخص کنید کدام سلول‌ها را می‌خواهید بخوانید.

```{r}
read_excel("data/deaths.xlsx", range = "A5:F15")
#> # A tibble: 10 × 6
#>   Name           Profession   Age `Has kids` `Date of birth`     `Date of death`
#>   <chr>          <chr>      <dbl> <lgl>      <dttm>              <dttm>         
#> 1 David Bowie    musician      69 TRUE       1947-01-08 00:00:00 2016-01-10 00:…
#> 2 Carrie Fisher  actor         60 TRUE       1956-10-21 00:00:00 2016-12-27 00:…
#> 3 Chuck Berry    musician      90 TRUE       1926-10-18 00:00:00 2017-03-18 00:…
#> 4 Bill Paxton    actor         61 TRUE       1955-05-17 00:00:00 2017-02-25 00:…
#> 5 Prince         musician      57 TRUE       1958-06-07 00:00:00 2016-04-21 00:…
#> 6 Alan Rickman   actor         69 FALSE      1946-02-21 00:00:00 2016-01-14 00:…
#> # ℹ 4 more rows
```

### ۵.۲.۲۰ انواع داده

در فایل‌های CSV، تمام مقادیر رشته‌ای هستند. این بسیار ساده است اما به این معنی است که شما باید کار اضافی برای تبدیل متغیرهایی که باید عددی باشند به اعداد انجام دهید.

اکسل این کار را متفاوت انجام می‌دهد. هنگام ورود داده به اکسل، اکسل سعی می‌کند نوع را برای هر ستون حدس بزند. در تلاش‌های خود برای "کمک"، اغلب اوقات اکسل کارهای کمتر مفیدی انجام می‌دهد، مانند تبدیل نام ژن‌ها به تاریخ (https://www.theverge.com/2020/8/6/21355674/human-genes-rename-microsoft-excel-misreading-dates).

همچنین می‌توانید برخی از این رفتارها را هنگام بارگذاری داده‌ها از اکسل به R مشاهده کنید. به عنوان مثال:

```{r}
df <- tibble(
  x = c(TRUE, FALSE, "TRUE"),
  y = c(1, 0, "1"),
  z = c("1.5", "1", "0.5")
)
df
#> # A tibble: 3 × 3
#>   x     y     z    
#>   <lgl> <dbl> <chr>
#> 1 TRUE      1 1.5  
#> 2 FALSE     0 1    
#> 3 NA        1 0.5
```

readxl شامل یک کاربرگ نمونه به نام `clippy.xlsx` است که می‌توانید با کد زیر بارگذاری کنید:

```{r}
xlsx_example <- readxl_example("clippy.xlsx")
```

شکل ۲۰.۳ نشان می‌دهد چگونه این می‌تواند در اکسل به نظر برسد:

بیایید بخوانیم و ببینیم چه می‌شود:

```{r}
read_excel(xlsx_example)
#> # A tibble: 4 × 3
#>   logical numeric date               
#>   <lgl>     <dbl> <dttm>             
#> 1 TRUE          1 1900-01-01 00:00:00
#> 2 FALSE         2 1900-01-02 00:00:00
#> 3 FALSE         3 1900-01-03 00:00:00
#> 4 TRUE          4 1900-01-04 00:00:00
```

ستون `logical` به عنوان منطقی شناسایی شده است. `numeric` عددی است و `date` تاریخ است.

می‌توانید از آرگومان `col_types` برای مشخص کردن نوع داده برای هر ستون استفاده کنید. انواع ممکن عبارتند از: `"skip"`, `"guess"`, `"logical"`, `"numeric"`, `"date"`, `"text"` یا `"list"`.

```{r}
read_excel(
  xlsx_example,
  col_types = c("text", "text", "text")
)
#> # A tibble: 4 × 3
#>   logical numeric date     
#>   <chr>   <chr>   <chr>    
#> 1 TRUE    1       39448    
#> 2 FALSE   2       39449    
#> 3 FALSE   3       39450    
#> 4 TRUE    4       39451
```

### ۶.۲.۲۰ نوشتن در اکسل

بیایید یک data frame کوچک ایجاد کنیم که بتوانیم آن را بنویسیم. توجه کنید که `item` یک فاکتور و `quantity` یک عدد صحیح است.

```{r}
bake_sale <- tibble(
  item     = factor(c("brownie", "cupcake", "cookie")),
  quantity = c(10, 5, 8)
)

bake_sale
#> # A tibble: 3 × 2
#>   item    quantity
#>   <fct>      <dbl>
#> 1 brownie       10
#> 2 cupcake        5
#> 3 cookie         8
```

می‌توانید داده‌ها را با استفاده از تابع `write_xlsx()` از بسته writexl به عنوان یک فایل اکسل به دیسک بنویسید:

```{r}
write_xlsx(bake_sale, path = "data/bake-sale.xlsx")
```

شکل ۲۰.۴ نشان می‌دهد داده‌ها در اکسل چگونه به نظر می‌رسند. توجه کنید که نام ستون‌ها شامل شده‌اند و به صورت پررنگ هستند. اینها را می‌توان با تنظیم آرگومان‌های `col_names` و `format_headers` روی `FALSE` خاموش کرد.

درست مانند خواندن از CSV، هنگام خواندن مجدد داده‌ها، اطلاعات مربوط به نوع داده از دست می‌رود. این باعث می‌شود که فایل‌های اکسل برای ذخیره‌سازی نتایج میانی قابل اعتماد نباشند. برای جایگزین‌ها، به بخش ۷.۵ مراجعه کنید.

```{r}
read_excel("data/bake-sale.xlsx")
#> # A tibble: 3 × 2
#>   item    quantity
#>   <chr>      <dbl>
#> 1 brownie       10
#> 2 cupcake        5
#> 3 cookie         8
```

### ۷.۲.۲۰ خروجی قالب‌بندی شده

بسته writexl یک راه‌حل سبک‌وزن برای نوشتن یک صفحه گسترده ساده اکسل است، اما اگر به ویژگی‌های اضافی مانند نوشتن به کاربرگ‌های درون یک صفحه گسترده و استایل‌دهی علاقه‌مند هستید، خواهید خواست از بسته openxlsx استفاده کنید. ما در اینجا به جزئیات استفاده از این بسته نمی‌پردازیم، اما توصیه می‌کنیم https://ycphs.github.io/openxlsx/articles/Formatting.html را برای بحث گسترده در مورد عملکرد قالب‌بندی بیشتر برای داده‌های نوشته شده از R به اکسل با openxlsx بخوانید.

توجه کنید که این بسته بخشی از tidyverse نیست، بنابراین توابع و گردش‌های کاری ممکن است آشنا نباشند. به عنوان مثال، نام توابع camelCase هستند، چندین تابع نمی‌توانند در پایپ‌لاین‌ها ترکیب شوند و آرگومان‌ها به ترتیبی متفاوت از آنچه معمولاً در tidyverse هستند قرار دارند. با این حال، این مشکلی ندارد. همانطور که یادگیری و استفاده از R شما خارج از این کتاب گسترش می‌یابد، با استایل‌های مختلف زیادی که در بسته‌های مختلف R استفاده می‌شوند برای دستیابی به اهداف خاص در R مواجه خواهید شد. یک راه خوب برای آشنایی با استایل کدنویسی مورد استفاده در یک بسته جدید، اجرای مثال‌های ارائه شده در مستندات تابع است تا حس خوبی از نحو و فرمت‌های خروجی و همچنین خواندن هر گونه vignette که ممکن است با بسته ارائه شود، بدست آورید.

### ۸.۲.۲۰ تمرین‌ها

1. در یک فایل اکسل، مجموعه داده زیر را ایجاد کنید و آن را به عنوان `survey.xlsx` ذخیره کنید. یا می‌توانید آن را به عنوان یک فایل اکسل از اینجا دانلود کنید.

سپس، آن را در R بخوانید، با `survey_id` به عنوان یک متغیر کاراکتری و `n_pets` به عنوان یک متغیر عددی.

```{r}
#> # A tibble: 6 × 2
#>   survey_id n_pets
#>   <chr>      <dbl>
#> 1 1              0
#> 2 2              1
#> 3 3             NA
#> 4 4              2
#> 5 5              2
#> 6 6             NA
```

2. در یک فایل اکسل دیگر، مجموعه داده زیر را ایجاد کنید و آن را به عنوان `roster.xlsx` ذخیره کنید. یا می‌توانید آن را به عنوان یک فایل اکسل از اینجا دانلود کنید.

سپس، آن را در R بخوانید. data frame حاصل باید `roster` نامیده شود و باید به شکل زیر باشد.

```{r}
#> # A tibble: 12 × 3
#>    group subgroup    id
#>    <dbl> <chr>    <dbl>
#>  1     1 A            1
#>  2     1 A            2
#>  3     1 A            3
#>  4     1 B            4
#>  5     1 B            5
#>  6     1 B            6
#>  7     1 B            7
#>  8     2 A            8
#>  9     2 A            9
#> 10     2 B           10
#> 11     2 B           11
#> 12     2 B           12
```

3. در یک فایل اکسل جدید، مجموعه داده زیر را ایجاد کنید و آن را به عنوان `sales.xlsx` ذخیره کنید. یا می‌توانید آن را به عنوان یک فایل اکسل از اینجا دانلود کنید.

   a. `sales.xlsx` را بخوانید و به عنوان `sales` ذخیره کنید. data frame باید به شکل زیر باشد، با `id` و `n` به عنوان نام ستون‌ها و با ۹ سطر.

```{r}
#> # A tibble: 9 × 2
#>   id      n    
#>   <chr>   <chr>
#> 1 Brand 1 n    
#> 2 1234    8    
#> 3 8721    2    
#> 4 1822    3    
#> 5 Brand 2 n    
#> 6 3333    1    
#> 7 2156    3    
#> 8 3987    6    
#> 9 3216    5
```

   b. `sales` را بیشتر اصلاح کنید تا آن را به فرمت تمیز زیر با سه ستون (`brand`، `id` و `n`) و ۷ سطر داده برسانید. توجه کنید که `id` و `n` عددی هستند، `brand` یک متغیر کاراکتری است.

```{r}
#> # A tibble: 7 × 3
#>   brand      id     n
#>   <chr>   <dbl> <dbl>
#> 1 Brand 1  1234     8
#> 2 Brand 1  8721     2
#> 3 Brand 1  1822     3
#> 4 Brand 2  3333     1
#> 5 Brand 2  2156     3
#> 6 Brand 2  3987     6
#> 7 Brand 2  3216     5
```

4. data frame `bake_sale` را دوباره ایجاد کنید، آن را با استفاده از تابع `write.xlsx()` از بسته openxlsx به یک فایل اکسل بنویسید.

5. در فصل ۷ در مورد تابع `janitor::clean_names()` برای تبدیل نام ستون‌ها به snake case یاد گرفتید. فایل `students.xlsx` را که قبلاً در این بخش معرفی کردیم بخوانید و از این تابع برای "تمیز کردن" نام ستون‌ها استفاده کنید.

6. اگر سعی کنید فایلی با پسوند `.xlsx` را با `read_xls()` بخوانید چه اتفاقی می‌افتد؟

## ۳.۲۰ Google Sheets

Google Sheets یکی دیگر از برنامه‌های صفحه گسترده پرکاربرد است. این رایگان و مبتنی بر وب است. درست مانند اکسل، در Google Sheets داده‌ها در کاربرگ‌ها (که sheets نیز نامیده می‌شوند) درون فایل‌های صفحه گسترده سازماندهی می‌شوند.

### ۱.۳.۲۰ پیش‌نیازها

این بخش نیز بر روی صفحات گسترده تمرکز خواهد داشت، اما این بار شما داده‌ها را از یک Google Sheet با بسته **googlesheets4** بارگذاری خواهید کرد. این بسته نیز جزو هسته tidyverse نیست، باید آن را به صورت صریح بارگذاری کنید.

```{r}
library(googlesheets4)
library(tidyverse)
```

یک یادداشت سریع در مورد نام بسته: googlesheets4 از v4 API Sheets استفاده می‌کند تا یک رابط R به Google Sheets ارائه دهد، از این رو نام آن.

### ۲.۳.۲۰ شروع کار

تابع اصلی بسته googlesheets4 `read_sheet()` است که یک Google Sheet را از یک URL یا یک شناسه فایل می‌خواند. این تابع همچنین با نام `range_read()` شناخته می‌شود.

شما می‌توانید یک Google Sheet عمومی را بدون احراز هویت با حساب Google خود بخوانید، با استفاده از `gs4_deauth()`.

```{r}
gs4_deauth()
```

برای خواندن یک Google Sheet، می‌توانید URL آن را ارائه دهید.

```{r}
students_sheet_id <- "1V1nPp1tzOuutXFLb3G9Eyxi3qxeEhnOXUzL5_BcCQ0w"
students <- read_sheet(students_sheet_id)
```

درست مانند آنچه با `read_excel()` انجام دادیم، می‌توانیم نام ستون‌ها، رشته‌های NA و انواع ستون را به `read_sheet()` ارائه دهیم.

```{r}
students <- read_sheet(
  students_sheet_id,
  col_names = c("student_id", "full_name", "favourite_food", "meal_plan", "age"),
  skip = 1,
  na = c("", "N/A"),
  col_types = "dcccc"
)
#> ✔ Reading from students.
#> ✔ Range 2:10000000.

students
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

توجه کنید که انواع ستون را کمی متفاوت در اینجا تعریف کردیم، با استفاده از کدهای کوتاه. به عنوان مثال، `"dcccc"` مخفف "double, character, character, character, character" است.

همچنین امکان خواندن کاربرگ‌های جداگانه از Google Sheets نیز وجود دارد. بیایید کاربرگ "Torgersen Island" را از Google Sheet penguins بخوانیم:

```{r}
penguins_sheet_id <- "1aFu8lnD_g0yjF5O-K6SFgSEWiHPpgvFCF0NY9D6LXnY"
read_sheet(penguins_sheet_id, sheet = "Torgersen Island")
#> ✔ Reading from penguins.
#> ✔ Range ''Torgersen Island''.
#> # A tibble: 52 × 8
#>   species island    bill_length_mm bill_depth_mm flipper_length_mm
#>   <chr>   <chr>     <list>         <list>        <list>           
#> 1 Adelie  Torgersen <dbl [1]>      <dbl [1]>     <dbl [1]>        
#> 2 Adelie  Torgersen <dbl [1]>      <dbl [1]>     <dbl [1]>        
#> 3 Adelie  Torgersen <dbl [1]>      <dbl [1]>     <dbl [1]>        
#> 4 Adelie  Torgersen <chr [1]>      <chr [1]>     <chr [1]>        
#> 5 Adelie  Torgersen <dbl [1]>      <dbl [1]>     <dbl [1]>        
#> 6 Adelie  Torgersen <dbl [1]>      <dbl [1]>     <dbl [1]>        
#> # ℹ 46 more rows
#> # ℹ 3 more variables: body_mass_g <list>, sex <chr>, year <dbl>
```

می‌توانید لیستی از تمام کاربرگ‌ها درون یک Google Sheet را با `sheet_names()` بدست آورید:

```{r}
sheet_names(penguins_sheet_id)
#> [1] "Torgersen Island" "Biscoe Island"    "Dream Island"
```

در نهایت، درست مانند `read_excel()`، می‌توانیم بخشی از یک Google Sheet را با تعریف `range` در `read_sheet()` بخوانیم. توجه کنید که ما همچنین از تابع `gs4_example()` در زیر برای یافتن یک Google Sheet نمونه که با بسته googlesheets4 ارائه می‌شود استفاده می‌کنیم.

```{r}
deaths_url <- gs4_example("deaths")
deaths <- read_sheet(deaths_url, range = "A5:F15")
#> ✔ Reading from deaths.
#> ✔ Range A5:F15.

deaths
#> # A tibble: 10 × 6
#>   Name          Profession   Age `Has kids` `Date of birth`    
#>   <chr>         <chr>      <dbl> <lgl>      <dttm>             
#> 1 David Bowie   musician      69 TRUE       1947-01-08 00:00:00
#> 2 Carrie Fisher actor         60 TRUE       1956-10-21 00:00:00
#> 3 Chuck Berry   musician      90 TRUE       1926-10-18 00:00:00
#> 4 Bill Paxton   actor         61 TRUE       1955-05-17 00:00:00
#> 5 Prince        musician      57 TRUE       1958-06-07 00:00:00
#> 6 Alan Rickman  actor         69 FALSE      1946-02-21 00:00:00
#> # ℹ 4 more rows
#> # ℹ 1 more variable: `Date of death` <dttm>
```

### ۳.۳.۲۰ خواندن کاربرگ‌های جداگانه

امکان خواندن کاربرگ‌های جداگانه از Google Sheets وجود دارد. آرگومان `sheet` را می‌توانید برای مشخص کردن کدام کاربرگ باید خوانده شود استفاده کنید. پیش‌فرض این است که اولین کاربرگ (مرئی) خوانده می‌شود.

```{r}
read_sheet(penguins_sheet_id, sheet = "Biscoe Island")
```

### ۴.۳.۲۰ نوشتن در Google Sheets

می‌توانید از R به Google Sheets با `write_sheet()` بنویسید. اولین آرگومان data frame برای نوشتن است، و دومین آرگومان نام (یا شناسه دیگر) Google Sheet برای نوشتن به آن است:

```{r}
write_sheet(bake_sale, ss = "bake-sale")
```

اگر می‌خواهید داده‌های خود را به یک کاربرگ خاص درون یک Google Sheet بنویسید، می‌توانید آن را با آرگومان `sheet` نیز مشخص کنید.

```{r}
write_sheet(bake_sale, ss = "bake-sale", sheet = "Sales")
```

### ۵.۳.۲۰ احراز هویت

در حالی که می‌توانید از یک Google Sheet عمومی بدون احراز هویت با حساب Google خود و با `gs4_deauth()` بخوانید، خواندن یک sheet خصوصی یا نوشتن به یک sheet نیاز به احراز هویت دارد تا googlesheets4 بتواند Google Sheets شما را مشاهده و مدیریت کند.

هنگامی که سعی می‌کنید یک sheet را که نیاز به احراز هویت دارد بخوانید، googlesheets4 شما را به یک مرورگر وب با درخواست ورود به حساب Google خود و اعطای مجوز برای کار از طرف شما با Google Sheets هدایت می‌کند. با این حال، اگر می‌خواهید یک حساب Google خاص، محدوده احراز هویت و غیره را مشخص کنید، می‌توانید این کار را با `gs4_auth()` انجام دهید، به عنوان مثال، `gs4_auth(email = "mine@example.com")`، که استفاده از توکن مرتبط با یک ایمیل خاص را اجباری می‌کند. برای جزئیات بیشتر احراز هویت، توصیه می‌کنیم مستندات googlesheets4 auth vignette را بخوانید: https://googlesheets4.tidyverse.org/articles/auth.html.

### ۶.۳.۲۰ تمرین‌ها

1. مجموعه داده `students` را که قبلاً در این فصل بود از اکسل و همچنین از Google Sheets بخوانید، بدون ارائه آرگومان‌های اضافی به توابع `read_excel()` و `read_sheet()`. آیا data frameهای حاصل در R کاملاً یکسان هستند؟ اگر نه، چه تفاوتی دارند؟

2. Google Sheet با عنوان survey را از https://pos.it/r4ds-survey بخوانید، با `survey_id` به عنوان یک متغیر کاراکتری و `n_pets` به عنوان یک متغیر عددی.

3. Google Sheet با عنوان roster را از https://pos.it/r4ds-roster بخوانید. data frame حاصل باید `roster` نامیده شود و باید به شکل زیر باشد.

```{r}
#> # A tibble: 12 × 3
#>    group subgroup    id
#>    <dbl> <chr>    <dbl>
#>  1     1 A            1
#>  2     1 A            2
#>  3     1 A            3
#>  4     1 B            4
#>  5     1 B            5
#>  6     1 B            6
#>  7     1 B            7
#>  8     2 A            8
#>  9     2 A            9
#> 10     2 B           10
#> 11     2 B           11
#> 12     2 B           12
```

## ۴.۲۰ خلاصه

Microsoft Excel و Google Sheets دو مورد از محبوب‌ترین سیستم‌های صفحه گسترده هستند. توانایی تعامل با داده‌های ذخیره شده در فایل‌های اکسل و Google Sheets به طور مستقیم از R یک ابرقدرت است! در این فصل یاد گرفتید چگونه داده‌ها را به R از صفحات گسترده اکسل با `read_excel()` از بسته readxl و از Google Sheets با `read_sheet()` از بسته googlesheets4 بخوانید. این توابع بسیار شبیه به یکدیگر کار می‌کنند و آرگومان‌های مشابهی برای مشخص کردن نام ستون‌ها، رشته‌های NA، سطرهایی که باید در بالای فایل که می‌خوانید رد شوند، و غیره دارند. علاوه بر این، هر دو تابع امکان خواندن یک کاربرگ واحد از یک صفحه گسترده را نیز فراهم می‌کنند.

از طرف دیگر، نوشتن به یک فایل اکسل نیاز به یک بسته و تابع متفاوت (`writexl::write_xlsx()`) دارد در حالی که می‌توانید با بسته googlesheets4 به یک Google Sheet بنویسید، با `write_sheet()`.

در فصل بعد، در مورد یک منبع داده متفاوت و نحوه خواندن داده‌ها از آن منبع به R یاد خواهید گرفت: پایگاه‌های داده.
