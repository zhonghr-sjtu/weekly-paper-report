# Weekly Paper Report

`weekly-paper-report` is a configurable Codex skill for research groups that want a two-stage weekly paper digest workflow:

1. Screen recent high-impact papers from user-defined research directions and produce a DOI/download list.
2. After PDFs are downloaded, generate an A4 newspaper-style weekly report image with representative figures, structured summaries, and DOI footers.

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

## Stage 2: Generate Report

After downloading PDFs, ask:

```text
PDFs are downloaded in WeeklyPaper. Use weekly-paper-report to generate the weekly report.
```

Codex will extract PDF metadata/text, crop representative figures, write summaries, and export an A4 PNG plus editable HTML.

## Automation

See [automation/monday-8am-workflow.md](automation/monday-8am-workflow.md) for a Monday 8:00 workflow pattern. The notification channel depends on your environment. Codex app notifications work by default where supported; SMS/iMessage requires a user-provided script or API.

## License

MIT License. See [LICENSE](LICENSE).
