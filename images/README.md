# Images Directory - راهنمای پوشه تصاویر

این پوشه شامل تمام تصاویر و نمودارهای مورد استفاده در ترجمه فارسی کتاب R for Data Science است.

## ساختار نام‌گذاری

تصاویر باید با الگوی زیر نام‌گذاری شوند:
- `ch{chapter}-L{lesson}-{description}.png`
- مثال: `ch1-L1-scatterplot.png` (فصل 1، درس 1، نمودار پراکندگی)

## تصاویر فصل 1 - درس 1 (Data Visualization)

برای درس 1 از فصل 1 (تصویرسازی داده‌ها)، تصاویر زیر مورد نیاز است:

### تصاویر مورد نیاز از https://r4ds.hadley.nz/data-visualize.html:
- [ ] `ch1-L1-first-plot.png` - اولین نمودار پراکندگی با displ و hwy
- [ ] `ch1-L1-mpg-dataset.png` - نمای مجموعه داده mpg
- [ ] `ch1-L1-aesthetic-color.png` - مثال mapping رنگ
- [ ] `ch1-L1-aesthetic-size.png` - مثال mapping اندازه
- [ ] `ch1-L1-aesthetic-alpha.png` - مثال mapping شفافیت
- [ ] `ch1-L1-aesthetic-shape.png` - مثال mapping شکل
- [ ] `ch1-L1-facet-wrap.png` - مثال facet_wrap
- [ ] `ch1-L1-facet-grid.png` - مثال facet_grid
- [ ] `ch1-L1-geom-point.png` - مثال geom_point
- [ ] `ch1-L1-geom-smooth.png` - مثال geom_smooth
- [ ] `ch1-L1-multiple-geoms.png` - ترکیب چند geom

## نحوه استفاده در فایل‌های Markdown

```markdown
![توضیح فارسی تصویر](images/ch1-L1-filename.png)
```

## منابع تصاویر

تمام تصاویر باید از منبع اصلی کتاب یا با اجرای کدهای R مربوطه تولید شوند.
- منبع: https://r4ds.hadley.nz/data-visualize.html
- لایسنس: مطابق با لایسنس کتاب اصلی (CC BY-NC-ND 4.0)

## دستورالعمل برای افزودن تصاویر جدید

1. تصویر را با کیفیت مناسب (PNG format) تهیه کنید
2. نام فایل را مطابق الگوی بالا انتخاب کنید
3. فایل را در این پوشه قرار دهید
4. در فایل markdown مربوطه با استفاده از مسیر نسبی ارجاع دهید
5. توضیح فارسی مناسبی برای alt text تصویر بنویسید
