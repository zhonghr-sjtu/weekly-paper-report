# Monday 8:00 Automation Pattern

This workflow has two stages.

## Stage 1: Automated DOI Screening

Schedule: every Monday at 08:00 local time.

Prompt template:

```text
Use weekly-paper-report to run Stage 1 DOI Screening.

Field:
{FIELD_NAME}

Research directions:
{RESEARCH_DIRECTIONS}

Target journals or venues:
{TARGET_JOURNALS}

Time window:
Prefer the last month. Broaden to 2-3 months only if a direction has too few strong candidates.

Output:
Return 10-12 papers with direction, title, venue, DOI/stable URL, and one-line selection reason.

Notification:
After screening, notify the user:
"论文筛选已完成，请下载 DOI 清单中的论文 PDF 到 WeeklyPaper 文件夹。下载完成后告诉我，我将生成本周周报。"
```

## Stage 2: User-Triggered Report Generation

After the user downloads PDFs:

```text
PDFs are downloaded in WeeklyPaper. Use weekly-paper-report to run Stage 2 and generate the weekly report.
```

## Notification Notes

Codex app automation/thread notification can be used where available. True SMS, iMessage, Slack, or email requires a user-provided integration script or API. Add the notification call after Stage 1 completes.
