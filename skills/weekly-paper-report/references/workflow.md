# Weekly Research Paper Report Workflow Notes

This reference captures practical implementation details validated during repeated weekly runs.

## Recommended Folder Structure

Under the weekly working directory, prefer:

- `text/` for first pages or short intake text
- `text_full/` for full-text extraction
- `page_renders/` for rendered PDF pages
- `raw_images/` for images extracted with `pdfimages`
- `crops/` for approved candidate figures

Example:

```text
WeeklyPaper/
  Week6/
    text/
    text_full/
    page_renders/
    raw_images/
    crops/
    figure_review.md
    codex_weekly_paper_express_YYYY-MM-DD_weekX_draft.html
    codex_weekly_paper_express_YYYY-MM-DD_weekX_draft.png
    codex_weekly_paper_express_YYYY-MM-DD_weekX_final.html
    codex_weekly_paper_express_YYYY-MM-DD_weekX_final.png
```

## Useful Commands

- `pdfinfo file.pdf`
- `pdftotext -layout -f 1 -l 4 file.pdf out.txt`
- `pdftotext -layout file.pdf out_full.txt`
- `pdftoppm -png -r 120 -f 1 -l 8 file.pdf render/page`
- `pdfimages -all file.pdf raw/img`
- `sips -g pixelWidth -g pixelHeight image.png`

## DOI And PDF Intake Rules

- Do not trust filenames alone.
- Deduplicate by DOI or by exact title when the same paper appears under multiple filenames.
- Record user exclusions explicitly, especially:
  - papers already used in prior weekly reports
  - journals the user considers below target tier
- When restoring the final paper count, prefer a replacement paper that:
  - fits the missing direction
  - has not appeared in prior weeks
  - is likely to yield a clean representative figure

## Figure Selection Heuristics

Prefer:

- graphical abstracts
- overview schematics
- workflow/model architecture figures
- phase/property maps
- representative microscopy or diffraction figures
- main result figures with a clear story

Avoid:

- pages dominated by text
- figures whose captions are inseparable from the crop
- images with excessive blank margins
- tiny dense multipanel figures when a cleaner panel exists

## Layout QA Checklist

When generating the A4 draft, check:

1. Does any figure bottom cross into the DOI band?
2. Was a crop replaced by the user with a new aspect ratio that no longer matches the old figure container?
3. Does any figure show white borders because the old aspect ratio was reused?
4. After enlarging text, do any figures need to shrink locally?
5. Is the DOI line readable and fully separated from the figure above it?

Preferred fix order:

1. Adjust the affected card's figure width or aspect ratio.
2. Re-render and inspect only the changed cards first.
3. Apply global resizing only if many cards fail simultaneously.

## Figure Review File

Before Stage 3, create a short `figure_review.md` that includes:

- current unique paper count
- duplicate or excluded papers
- one candidate image filename per paper
- journal and DOI
- asset folder locations

This makes user review faster and helps preserve state between runs.
