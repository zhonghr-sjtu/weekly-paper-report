---
name: weekly-paper-report
description: Use this skill to run a configurable weekly research-paper digest workflow for any academic field. It first screens recent high-impact papers and produces a DOI/download list based on the user's prompt, research directions, target journals, time window, and language preferences; then, after the user downloads PDFs, it generates an A4 newspaper-style report image with representative figures, structured summaries, and DOI footers. Trigger for requests about 每周论文速递, 论文周报, DOI筛选, paper digest, literature weekly report, WeeklyPaper, or automated paper screening.
metadata:
  short-description: Configurable DOI screening, figure review, and A4 research paper weekly report
---

# Weekly Paper Report

This skill supports a gated multi-stage paper digest workflow for any research group or academic field.

## Workflow Update

This skill was upgraded from an older two-stage workflow to a richer staged workflow validated in repeated weekly materials-science runs.

- Old workflow:
  - `Stage 1`: DOI screening
  - `Stage 2`: directly generate the A4 report after PDFs are downloaded
- Updated workflow:
  - `Stage 1`: DOI screening
  - `Stage 2`: PDF intake and representative figure extraction
  - `Stage 3`: A4 draft layout generation after figure review
  - `Stage 4`: text revision and finalization

The key change is that the old `Stage 2` was split into a new `Stage 2` and `Stage 3`, so figure quality can be reviewed before report layout. This is more robust for weekly paper digests that depend on heterogeneous figure aspect ratios, user-replaced crops, and dense A4 layouts.

## Core Principle

Do not assume the field is materials science. Derive the research scope from the user's prompt.

If the prompt provides research directions, target journals, candidate keywords, output language, or paper count, use them. If any essential scope is missing, make a conservative inference from the user's project context and state the assumption. Ask a question only when the field cannot be inferred.

## User Scope Extraction

Before Stage 1, identify:

- `field_name`: broad field, e.g. materials science, neuroscience, oncology, robotics, climate science.
- `research_directions`: user-provided directions or 4-8 inferred subtopics.
- `target_journals`: user-provided journals or field-appropriate high-impact journals.
- `time_window`: default last month; broaden to 2-3 months if coverage is weak.
- `paper_count`: default 10-12 DOI candidates for Stage 1; default 10 papers for the final A4 report.
- `output_language`: default Chinese if the user uses Chinese; otherwise follow the user's language.
- `download_folder`: default `WeeklyPaper/` under the current project directory unless the user specifies another path.

## Journal Selection

If the user provides journals, prioritize them.

If journals are not provided, infer authoritative journals for the field. Examples:

- General science: Nature, Science, PNAS, Nature Communications, Science Advances.
- Materials/chemistry: Nature Materials, Advanced Materials, Acta Materialia, ACS Nano, JACS, Angewandte Chemie, Chemistry of Materials.
- Biology/medicine: Nature Medicine, NEJM, The Lancet, JAMA, Cell, Nature Biotechnology, Nature Genetics.
- AI/CS/robotics: Nature Machine Intelligence, Science Robotics, ICML, NeurIPS, ICLR, CVPR, ACL, IEEE/ACM Transactions, RSS/CoRL.
- Physics: Physical Review Letters, Nature Physics, Science, PRX, Physical Review journals.
- Earth/climate: Nature Climate Change, Nature Geoscience, Science, PNAS, Environmental Research Letters.

For niche fields, use high-quality specialty journals and conferences when they are more relevant than general journals.

## Stage 1: DOI Screening

When the user asks to start screening or an automation runs:

1. Parse the user's scope and produce a short internal search plan.
2. Search broadly across publisher pages, DOI pages, Crossref/PubMed/arXiv when applicable, Google Scholar-style web results, and society/conference pages.
3. Select 10-12 candidate papers unless the user requests another number.
4. Balance across `research_directions`; aim for 1-2 papers per direction.
5. Prefer papers that are recent, high-impact, on-topic, and likely to contain clear representative figures.
6. Return a concise DOI download list with:
   - direction/category
   - paper title
   - journal/conference
   - DOI URL or stable URL
   - one-line reason for selection
7. Ask the user to download PDFs into `download_folder`, then notify Codex to start Stage 2.

Do not generate the report in Stage 1 unless PDFs are already present and the user asks for Stage 2.

## Stage 2: PDF Intake And Figure Extraction

When the user says PDFs are downloaded:

1. Locate PDFs in `download_folder`.
2. Extract metadata and text using local tools such as `pdfinfo`, `pdftotext`, `pdftoppm`, and the bundled Python runtime.
3. Confirm title, DOI, source, authors, institution, and corresponding author where possible.
4. Choose the final papers, preserving direction balance and figure quality.
5. Detect duplicate PDFs when a folder contains multiple filenames for the same paper. Prefer DOI/title deduplication over filename-based assumptions.
6. Respect user exclusions from prior weeks or journal-quality feedback. If a paper was previously used in a weekly report, or the user explicitly rejects a journal tier, exclude it from the final set and record the preference for future runs.
7. Render PDF pages and extract representative figures:
   - Prefer graphical abstract, Fig. 1, overview schematic, key experimental/analysis figure, or main result figure.
   - Avoid cropped text blocks, figure captions, page headers, or incomplete panels.
   - Prefer complete figures first; if a full figure is too dense, extract the dominant panel or a clean subset of panels.
   - Preserve original image aspect ratio as much as possible; do not stretch images to fit a generic box.
8. Save figure assets into dedicated folders under `download_folder`, for example:
   - `crops/` for candidate figures intended for layout
   - `page_renders/` for page-level renders
   - `raw_images/` for embedded images extracted from the PDF
   - `text/` and `text_full/` for intake text
9. Return the figure asset folder path and ask the user to review or replace images before layout generation.

Required handoff text after Stage 2:

`代表图片已提取，请先审核 {figure_asset_folder} 中的图片；如果你修改了其中某些图片，也请直接覆盖或补充到该文件夹，确认后告诉我进入周报排版。`

Do not generate the A4 report before the user approves the figure set, unless the user explicitly asks for a draft anyway.

## Stage 3: Draft Report Layout

After the user confirms the figure set, or updates the extracted images:

1. Re-read the approved image assets from the figure folder instead of assuming the earlier crops are still correct.
2. Write each paper block in the output language.
3. Use the default summary style, but make the institution/team attribution explicit when possible.
4. Generate an A4 portrait draft in editable HTML plus rendered PNG.
5. Keep figure layout responsive to image shape:
   - wide images should use wider figure boxes
   - portrait images should keep portrait boxes
   - square images should stay near square
   - use `object-fit: contain` unless the user explicitly prefers tighter cropping
6. During draft layout QA, check for these recurrent failure modes:
   - image bottoms crossing into the DOI band
   - stale figure-box aspect ratios after the user replaces a crop
   - excessive white margins caused by reusing an old figure container ratio for a new image
   - figure size being too large for enlarged text blocks
7. If layout issues appear, first resize or re-ratio the affected figure block instead of globally shrinking all cards.
8. Notify the user that the draft is ready for text review.

Required handoff text after Stage 3:

`周报排版草稿已完成，请重点审查文字内容是否准确、饱满，以及机构团队表述是否合适；如需修改，请直接告诉我。`

## Stage 4: Text Revision And Finalization

When the user gives text or layout feedback:

1. Revise summaries, title lines, institution/team attribution, conclusions, and issue labels as requested.
2. If the text revision changes layout pressure, rebalance figure size and text density without breaking approved image proportions.
3. If the user replaces one specific crop, update only that figure block's aspect ratio and dimensions unless broader changes are needed.
4. Re-render the draft and repeat review if needed.
5. Only treat the report as final when the user explicitly approves the text and layout or clearly indicates no further changes are needed.

Required handoff text after final approval:

`最终版周报已完成。`

## Default Summary Style

For Chinese reports, use:

`（一句话概括研究背景），关键科学问题是（...）。近日，（机构）的（第一通讯作者）团队通过（方法/技术），针对（问题）开展研究，主要结论如下：（1）...；（2）...；（3）...。`

For English reports, use the equivalent structure:

`Background sentence. The key scientific question is ... Recently, the team led by ... at ... used ... to investigate ... Main conclusions: (1) ...; (2) ...; (3) ... .`

Prefer actual Conclusions/Discussion statements. If the paper lacks a Conclusions section, synthesize from abstract, final Discussion paragraphs, and major figure captions.

## Default Layout Rules

- A4 portrait, default 1240 x 1754 px.
- Ten blocks in a 2 x 5 grid unless the user requests another layout.
- Each block order: source/category line, title as an independent line, then figure plus summary text.
- Use image + text wrapping under the title when it improves density.
- Increase summary font size when readable space remains; keep DOI visible inside the block.
- Keep the top title visually separated from the top rule.
- No draft/process notes in the final report image.
- Do not force one fixed figure size across all papers when approved images have different aspect ratios.
- Protect the DOI band: figure bottoms must remain above the DOI footer after every layout revision.
- If the user enlarges text or requests denser summaries, shrink affected figure boxes before reducing global readability.
- When a new crop replaces an earlier one, update the corresponding figure-box aspect ratio to match the new image and avoid white borders.

Default Chinese title:
`Codex每周论文速递`

Default Chinese subtitle:
`本期关注：{research_directions joined by 、}`

## Automation Handoff

For scheduled automation, Stage 1 should output the DOI list and notify the user:

`论文筛选已完成，请下载 DOI 清单中的论文 PDF 到 {download_folder} 文件夹；下载完成后告诉我，我将先提取候选代表图片供你审核。`

After image extraction, notify the user:

`代表图片已提取，请先审核 {figure_asset_folder} 中的图片；如果你修改了其中某些图片，也请直接覆盖或补充到该文件夹，确认后告诉我进入周报排版。`

After draft layout, notify the user:

`周报排版草稿已完成，请重点审查文字内容是否准确、饱满，以及机构团队表述是否合适；如需修改，请直接告诉我。`

After final approval, notify the user:

`最终版周报已完成。`

If true SMS/iMessage integration is unavailable, use the Codex app automation/thread notification and state the limitation briefly. If a user-provided SMS script or API is available, call it after Stage 1 completes.

## References

For detailed templates, public packaging notes, and examples, read `references/workflow.md` only when implementing or revising the workflow.
