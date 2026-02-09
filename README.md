# Video Project Analyzer

AI-powered project management extraction from meeting video recordings using Google Gemini.

## What is this?

This repository contains structured project management analyses extracted from video meeting recordings. Using the `project_management` analysis mode of the [Instagram Video Analyzer MCP](https://github.com/Morfeu333/Video-Frame-Extraction-Utility), meeting recordings are processed by Google Gemini to automatically extract:

- Decisions made (with timestamps and owners)
- Action items / tasks (with responsible, priority, dependencies)
- Technical requirements (with stack, complexity, type)
- Architecture and infrastructure details
- Risks and blockers
- Business model and project scope
- Next steps
- Executive summary

## Analyses

| File | Meeting | Date | Duration |
|------|---------|------|----------|
| [PM Analysis](RCIS_Reuniao_INFRA_Saulo_2026-02-05_PM_Analysis.md) | RCIS Infra - Saulo | 2026-02-05 | 28min |
| [Visual Analysis](RCIS_Reuniao_INFRA_Saulo_2026-02-05_Visual_Analysis.md) | RCIS Infra - Saulo | 2026-02-05 | 28min |

## How it works

```
Meeting Video (.mp4) --> Gemini File API (upload) --> Gemini 2.5 Flash (analysis) --> Structured Markdown
```

### Analysis types available
- `comprehensive` - Full analysis (visual + audio + themes + timestamps + insights)
- `summary` - Quick summary
- `transcription` - Audio transcription with timestamps
- `visual_description` - Visual content description
- `project_management` - PM extraction (decisions, action items, requirements, risks, architecture)

### Usage via MCP
```bash
mcpl call instagram-video-analyzer analyze_local_video \
  '{"file_path": "/path/to/meeting.mp4", "analysis_type": "project_management"}'
```

## Tech Stack
- **AI Model**: Google Gemini 2.5 Flash
- **File API**: Gemini File API (up to 2GB per file)
- **MCP Server**: Python + FastMCP
- **Output**: Structured Markdown
