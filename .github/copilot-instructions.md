# Copilot Instructions for R4DS Persian Translation Project

## Repository Overview

This repository contains the Persian (Farsi) translation of the book "R for Data Science" (R4DS) by Hadley Wickham, Mine Çetinkaya-Rundel, and Garrett Grolemund. The translation is officially authorized by the original authors and is published under CC BY-NC-ND 4.0 license.

## Project Purpose

The goal is to provide a free, accurate, and reliable resource for learning R programming and data science in Persian language. This is a community-driven translation project where contributors translate and review chapters of the original book.

## Repository Structure

- **Chapter files**: Each lesson is in a separate Markdown file (e.g., `ch1-L4.md`, `ch2-L1.md`)
  - `ch1-L*`: Chapter 1 lessons (Data exploration and transformation)
  - `ch2-L*`: Chapter 2 lessons (Data visualization)
  - Naming pattern: `ch{chapter_number}-L{lesson_number}.md` or `ch{chapter_number}_L{lesson_number}.md`
- **Special files**:
  - `00-introduction.md`: Book introduction
  - `SUMMARY.md`: Table of contents and translation status tracking
  - `CONTRIBUTING.md`: Contribution guidelines for translators
  - `README.md`: Project overview and status
- **Assets**: `images/` directory contains figures and diagrams

## Language and Content Guidelines

### Persian Language Conventions

1. **Direction**: Content is in Persian (right-to-left script) but code blocks are in English (left-to-right)
2. **Technical terms**: 
   - R function names remain in English (e.g., `filter()`, `mutate()`, `ggplot()`)
   - Technical concepts use established Persian equivalents when available
   - Include original English term in parentheses when introducing new concepts
3. **Code comments**: Persian comments use `#` followed by Persian text

### Content Structure

1. **Headers**: Use Persian headers with proper numbering (e.g., `# درس 4: سبک کدنویسی`)
2. **Code blocks**: R code blocks should be marked with ````r` or `{r}` syntax
3. **Exercises**: Sections titled `تمرینها` contain practice exercises
4. **Cross-references**: Use `{#sec-name}` for section references

## Common Packages and Libraries

The book primarily uses these R packages:
- `tidyverse`: Collection of data science packages
- `nycflights13`: NYC flights dataset for examples
- `ggplot2`: Data visualization (part of tidyverse)
- `dplyr`: Data manipulation (part of tidyverse)

## Code Style Guidelines

The book teaches specific R coding style conventions:
- Pipe operator `|>` should have a space before it and be at the end of a line
- Proper spacing around operators
- Consistent indentation for multi-line pipelines
- Meaningful variable names

## Translation Workflow

1. Contributors select untranslated chapters from `SUMMARY.md`
2. Create a new branch for translation work
3. Translate content while maintaining:
   - Fidelity to original meaning
   - Fluency in Persian
   - Consistency with existing translations
4. Submit Pull Request for review
5. Track status in `SUMMARY.md` (✅ complete, ⏳ pending)

## Important Considerations for Copilot

1. **Preserve code blocks**: Never translate R code, function names, or dataset names
2. **Maintain formatting**: Keep markdown structure, code fence syntax, and YAML headers intact
3. **Respect RTL text**: Persian content flows right-to-left but code remains left-to-right
4. **Consistency**: Follow existing translation patterns for technical terms
5. **File naming**: Use existing naming conventions (e.g., `ch1-L4.md` format)
6. **No deletion**: This is a translation project - don't remove content unless fixing errors
7. **Examples**: Keep code examples and their output exactly as in original

## When Making Changes

- Focus on translation accuracy and language quality
- Ensure code blocks render correctly in Markdown
- Verify Persian text displays properly (RTL)
- Check cross-references and internal links
- Update `SUMMARY.md` status when completing sections
- Follow the contribution guidelines in `CONTRIBUTING.md`

## Testing and Validation

Since this is a documentation/translation project:
- Visual review of rendered Markdown is primary validation
- Check that code examples are syntactically correct
- Verify Persian text rendering
- Ensure links and references work correctly

## Questions or Issues

Contributors should open GitHub issues for:
- Translation questions
- Technical term standardization
- Structural changes
- New vocabulary additions
