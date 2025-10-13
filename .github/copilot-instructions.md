# Copilot Instructions for R4DS Persian Translation

## Project Overview

This repository contains the Persian (Farsi) translation of "R for Data Science" by Hadley Wickham, Mine Çetinkaya-Rundel, and Garrett Grolemund. The translation is officially authorized by the original authors.

## Language and Content Guidelines

### Primary Language
- **Content Language**: Persian (Farsi) - Right-to-Left (RTL)
- **Code**: R programming language
- **Code Comments**: Persian preferred where helpful, but English is acceptable for technical terms
- **Technical Terms**: Use established Persian equivalents when available; otherwise use English terms with Persian explanations

### Translation Standards
- Maintain fidelity to the original text while ensuring readability for Persian speakers
- Use consistent terminology throughout the book (refer to glossary when available)
- Preserve all R code blocks exactly as in the original
- Translate code comments to Persian where appropriate
- Keep function names, package names, and variable names in English as they appear in R

## File Structure and Naming

- Each chapter is in a separate Markdown file
- Naming convention: `ch[chapter]-L[lesson].md` or `ch[chapter]_L[lesson].md`
- Images are stored in the `images/` directory
- Table of contents is in `SUMMARY.md`
- Translation status is tracked in `README.md`

## Code Style Guidelines

### R Code Blocks
- Use proper R syntax and tidyverse conventions
- Follow the pipe operator style: `|>` should have a space before it and typically be the last thing on a line
- Maintain proper spacing around operators
- Use consistent indentation (2 spaces)
- Example from the book:
  ```r
  flights |>  
    filter(dest == "IAH") |> 
    group_by(year, month, day) |> 
    summarize(n = n(), delay = mean(arr_delay, na.rm = TRUE))
  ```

### Code Comments in Examples
- Code comments in examples should be in Persian
- Use `#` for comments as per R standard
- Keep comments concise and explanatory

## Markdown Formatting

- Use proper heading hierarchy (# for chapter titles, ## for sections, etc.)
- Code blocks should use triple backticks with language specification: ````r`
- Preserve all chunk options like `#| label:` and `#| message: false`
- Maintain consistent formatting for lists and tables

## Common Packages Used

The book primarily uses these R packages:
- `tidyverse` - collection of data science packages
- `nycflights13` - example dataset
- `ggplot2` - for data visualization (part of tidyverse)
- `dplyr` - for data manipulation (part of tidyverse)
- `ggrepel` - for label positioning in plots
- `scales` - for scale functions in plots

## Translation Status

- Check `README.md` for current translation status
- Chapters may be: ✅ Complete, ⏳ Awaiting Review, or ⏳ Awaiting Translation
- See `CONTRIBUTING.md` for contribution guidelines

## Best Practices

1. **Preserve Technical Accuracy**: R code must remain functional and accurate
2. **Maintain Consistency**: Use the same Persian terms for repeated concepts
3. **Keep Original Structure**: Maintain the original chapter and section structure
4. **Test Code Examples**: Ensure all R code examples work correctly
5. **Respect RTL Formatting**: Be mindful of right-to-left text flow for Persian content
6. **Reference Original**: When in doubt, refer to the original English version at https://r4ds.hadley.nz/

## Pull Request Guidelines

- Create a branch with a descriptive name (e.g., `chapter3-translation`)
- Ensure translation quality before submitting
- Check that all R code blocks are properly formatted
- Review for consistency with existing translations
- See `CONTRIBUTING.md` for detailed contribution guidelines

## Terminology Guidelines

- **Data**: داده / داده‌ها
- **Visualization**: تصویرسازی / تجسم داده
- **Pipe**: پایپ (pipeline)
- **Function**: تابع
- **Package**: بسته
- **Variable**: متغیر
- **Object**: شئ / شیء
- When translating function names or technical terms, include the English term in parentheses or code format on first use

## Special Considerations

- The book uses Quarto/R Markdown format - preserve all special syntax
- Some exercises contain intentionally incorrect code for teaching purposes - preserve these as-is
- Screenshots and figures should remain in English unless specifically translated
- Mathematical notation and formulas should remain as-is
- URLs and references should point to original sources unless Persian equivalents exist
