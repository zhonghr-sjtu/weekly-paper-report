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
- `WeeklyPaper/final_assets/`

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

## Final Deliverables

Return:

- rendered PNG link
- editable HTML link
- asset folder link

Mention excluded papers only if useful.
