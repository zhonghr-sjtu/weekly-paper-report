# Weekly Paper Report

`weekly-paper-report` is a configurable Codex skill for research groups that want a staged weekly paper digest workflow:

1. Screen recent high-impact papers from user-defined research directions and produce a DOI/download list.
2. After PDFs are downloaded, extract metadata and candidate representative figures for review.
3. After figure review, generate an A4 newspaper-style draft with representative figures, structured summaries, and DOI footers.
4. Revise text/layout if needed and finalize the weekly report.

## Workflow Update

This repository previously documented a simpler two-stage workflow:

1. `Stage 1`: DOI screening
2. `Stage 2`: generate the final report directly after PDFs are downloaded

It has now been updated to a richer staged workflow:

1. `Stage 1`: DOI screening
2. `Stage 2`: PDF intake and figure extraction
3. `Stage 3`: draft layout generation after figure review
4. `Stage 4`: text revision and finalization

The main change is that the old `Stage 2` has been split into the new `Stage 2` and `Stage 3`, so users can review, replace, or correct representative figures before A4 layout generation. This is especially important for weekly reports where image aspect ratios vary significantly and layout quality depends on good figure crops.

The workflow is field-agnostic. It does not hard-code materials science directions. Users provide a prompt such as:

> Please screen recent papers for our lab. Directions: computational neuroscience, neural decoding, brain-computer interfaces, foundation models for biology. Target journals: Nature, Science, Neuron, Nature Neuroscience, Cell, eLife, PNAS, ICLR, NeurIPS. Output in Chinese.

## Repository Layout

```text
skills/weekly-paper-report/
  SKILL.md
  agents/openai.yaml
  references/workflow.md
examples/
  materials-science.md
  biomedical.md
  ai-robotics.md
automation/
  monday-8am-workflow.md
```

## Install

Copy the skill folder into your Codex skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R skills/weekly-paper-report ~/.codex/skills/
```

Restart Codex or open a new session so the skill metadata is loaded.

## Stage 1: DOI Screening

Ask Codex to screen papers with your field-specific prompt. Example:

```text
Use weekly-paper-report to screen this week's papers.
Research directions: metal alloy design, AI for Materials, molecular dynamics, neutron characterization, additive manufacturing.
Target journals: Nature, Science, Nature Materials, Science Advances, Nature Communications, Acta Materialia, International Journal of Plasticity, Advanced Materials.
Return 10-12 DOI links.
```

Codex returns a DOI/download list. Download the PDFs into a folder such as:

```text
WeeklyPaper/
```

After screening, the expected handoff is:

```text
Screening is complete. Please download the PDFs from the DOI list into the WeeklyPaper folder. After that, tell me to start representative figure extraction for review.
```

## Stage 2: PDF Intake And Figure Extraction

After downloading PDFs, ask:

```text
PDFs are downloaded in WeeklyPaper. Use weekly-paper-report to run Stage 2.
```

Codex will:

- extract PDF metadata and text
- detect duplicate papers when filenames differ
- extract page renders and embedded images
- select one candidate representative figure per paper
- write a `figure_review.md` review note plus `crops/`, `page_renders/`, `raw_images/`, `text/`, and `text_full/` artifacts

The expected handoff after Stage 2 is:

```text
Representative figures have been extracted. Please review the images in {figure_asset_folder}. If you replace or add any images, overwrite or add them in that folder, then tell me to proceed to report layout.
```

## Stage 3: Draft Layout

After the user confirms the figure set, ask:

```text
Representative figures are approved in WeeklyPaper. Use weekly-paper-report to run Stage 3 and generate the draft weekly report.
```

Codex will generate:

- an editable HTML draft
- a rendered PNG draft
- a 2 × 5 A4 layout by default for 10 papers

Stage 3 also includes layout QA for:

- figure bottoms crossing into the DOI band
- white borders caused by stale aspect-ratio boxes
- card-specific figure resizing after the user replaces crops

The expected handoff after Stage 3 is:

```text
The weekly report draft is ready. Please review the text for accuracy, completeness, and team/institution attribution, and tell me about any revisions needed.
```

## Stage 4: Finalization

After the user confirms the draft or gives revision feedback, ask Codex to revise and finalize the weekly report.

Typical final outputs are:

- `..._final.html`
- `..._final.png`

The expected final handoff is:

```text
The final weekly report is complete.
```

## Practical Notes

- Do not rely on filenames alone; deduplicate by DOI or exact title when necessary.
- Track papers already used in prior weekly reports to avoid reuse when the user wants fresh coverage.
- Track journal-tier feedback from the user; if a venue is considered below target quality, exclude similar selections in future runs unless explicitly requested.
- When the user replaces a figure, update that card's aspect ratio instead of reusing the old figure box.

## Automation

See [automation/monday-8am-workflow.md](automation/monday-8am-workflow.md) for a Monday 8:00 workflow pattern. The notification channel depends on your environment. Codex app notifications work by default where supported; SMS/iMessage requires a user-provided script or API.

## License

MIT License. See [LICENSE](LICENSE).
