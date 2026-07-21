# Weekly Research Paper Report Checklist

## Stage 1 Scope Template

Ask or infer:

```yaml
field_name: ""
research_directions:
  - ""
target_journals:
  - ""
time_window: "last month"
paper_count: 10-12
output_language: "Chinese or user language"
download_folder: "WeeklyPaper/"
```

## Stage 1 DOI List Template

| Direction | Paper | Venue | DOI / URL | Why selected |
|---|---|---|---|---|
| Direction A | Title | Journal | https://doi.org/... | ... |

Return 10-12 papers by default. Mark papers outside the default time window as "time window broadened".

Before finalizing the Stage 1 list, check whether a historical workbook already exists in the project, preferably:

`outputs/weekly_paper_history/weekly_paper_covered_papers.xlsx`

If it exists:

- read the DOI/title history before selecting final candidates
- exclude papers already covered in previous weekly reports by DOI, DOI URL, or clearly equivalent title
- only keep a repeated paper when the user explicitly asks for a revisit

## Stage 2 File Workflow

Expected project folder:
`WeeklyPaper/`

Useful commands:

- `find WeeklyPaper -maxdepth 2 -type f | sort`
- `pdfinfo file.pdf`
- `pdftotext -layout file.pdf out.txt`
- `pdftoppm -png -r 100 -f 1 -l 12 file.pdf render/page`

Recommended generated folders:

- `WeeklyPaper/text/`
- `WeeklyPaper/text_full/`
- `WeeklyPaper/page_renders/`
- `WeeklyPaper/crops/`
- `WeeklyPaper/final_assets/`

## Stage 2 Figure Review Handoff

After extracting candidate figures, stop and ask the user to review the figure folder before report generation.

Recommended wording:

`代表图片已提取，请先审核 {figure_asset_folder} 中的图片；如果你修改了其中某些图片，也请直接覆盖或补充到该文件夹，确认后告诉我进入周报排版。`

Do not skip this checkpoint in the default workflow.

## Stage 3 Draft Layout

Once the user approves or updates the figure assets:

- re-read the actual image files from the figure folder
- preserve image aspect ratio as much as possible
- allow per-paper figure box sizing instead of forcing one shared image size
- generate editable HTML and rendered PNG
- ask the user to review the text before declaring the report final

Recommended wording:

`周报排版草稿已完成，请重点审查文字内容是否准确、饱满，以及机构团队表述是否合适；如需修改，请直接告诉我。`

## Stage 4 Final Approval

Only output the final version after the user approves the text.

After the final version is explicitly approved:

- update the historical workbook if it exists, preferably `outputs/weekly_paper_history/weekly_paper_covered_papers.xlsx`
- write this issue's final paper list into the workbook, including at least week label, report date, category, Chinese digest title, DOI, DOI URL, official title, journal, and final source file
- replace any temporary Stage 2 or draft-status records for the same issue with the final approved record rather than duplicating them

Recommended wording:

`最终版周报已完成。`

## Summary Style

Chinese:

`（一句话概括研究背景），关键科学问题是（...）。近日，（机构）的（第一通讯作者）团队通过（方法/技术），针对（问题）开展研究，主要结论如下：（1）...；（2）...；（3）...。`

English:

`Background sentence. The key scientific question is ... Recently, the team led by ... at ... used ... to investigate ... Main conclusions: (1) ...; (2) ...; (3) ... .`

Prefer actual Conclusions/Discussion statements. If the paper lacks a Conclusions section, synthesize from abstract, final Discussion paragraphs, and major figure captions.

## Figure Selection

Good figure types:

- graphical abstract
- overview schematic
- main result figure
- representative microscopy/visualization
- workflow/model architecture
- performance map/curve
- field-specific key evidence figure

Avoid:

- pure text pages
- incomplete panels
- page headers
- figure captions in final crop
- tiny unreadable multi-panel images unless they communicate the paper's story
- fixed-size stretching that distorts the approved image

## Final Deliverables

Return:

- rendered PNG link
- editable HTML link
- asset folder link

Mention excluded papers only if useful.
