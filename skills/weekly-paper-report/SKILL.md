---
name: weekly-paper-report
description: Use this skill to run a configurable weekly research-paper digest workflow for any academic field. It first screens recent high-impact papers and produces a DOI/download list based on the user's prompt, research directions, target journals, time window, and language preferences; then, after the user downloads PDFs, it generates an A4 newspaper-style report image with representative figures, structured summaries, and DOI footers. Trigger for requests about 每周论文速递, 论文周报, DOI筛选, paper digest, literature weekly report, WeeklyPaper, or automated paper screening.
metadata:
  short-description: Configurable DOI screening and A4 research paper weekly report
---

# Weekly Paper Report

This skill supports a two-stage paper digest workflow for any research group or academic field.

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

## Stage 2: PDF To A4 Weekly Report

When the user says PDFs are downloaded:

1. Locate PDFs in `download_folder`.
2. Extract metadata and text using local tools such as `pdfinfo`, `pdftotext`, `pdftoppm`, and the bundled Python runtime.
3. Confirm title, DOI, source, authors, institution, and corresponding author where possible.
4. Choose the final papers, preserving direction balance and figure quality.
5. Render PDF pages and crop representative figures:
   - Prefer graphical abstract, Fig. 1, overview schematic, key experimental/analysis figure, or main result figure.
   - Avoid cropped text blocks, figure captions, page headers, or incomplete panels.
   - Use `object-fit: contain` in final layout unless intentional cropping is needed.
6. Write each paper block in the output language.
7. Generate an A4 portrait PNG and editable HTML.

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

Default Chinese title:
`Codex每周论文速递`

Default Chinese subtitle:
`本期关注：{research_directions joined by 、}`

## Automation Handoff

For scheduled automation, Stage 1 should output the DOI list and notify the user:

`论文筛选已完成，请下载 DOI 清单中的论文 PDF 到 {download_folder} 文件夹。下载完成后告诉我，我将生成本周周报。`

If true SMS/iMessage integration is unavailable, use the Codex app automation/thread notification and state the limitation briefly. If a user-provided SMS script or API is available, call it after Stage 1 completes.

## References

For detailed templates, public packaging notes, and examples, read `references/workflow.md` only when implementing or revising the workflow.
