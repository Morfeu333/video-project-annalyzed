# Video Project Analyzer

AI-powered project management and candidate skills extraction from meeting video recordings using Google Gemini.

## What is this?

This repository contains structured analyses extracted from video meeting recordings. Using the [Instagram Video Analyzer MCP](https://github.com/Morfeu333/instagram-video-analyzer-mcp), meeting recordings are processed by Google Gemini to automatically extract:

- **Project Management**: Decisions, action items, requirements, risks, architecture
- **Candidate Skills**: Evidence-based technical proficiency scoring with source quotes
- **Visual Analysis**: Scene-by-scene visual content description
- **Transcription**: Timestamped audio transcription

## Analyses

### Project Management & Visual
| File | Meeting | Date | Duration |
|------|---------|------|----------|
| [PM Analysis](RCIS_Reuniao_INFRA_Saulo_2026-02-05_PM_Analysis.md) | RCIS Infra - Saulo | 2026-02-05 | 28min |
| [Visual Analysis](RCIS_Reuniao_INFRA_Saulo_2026-02-05_Visual_Analysis.md) | RCIS Infra - Saulo | 2026-02-05 | 28min |

### Vibecoder Candidate Profiles
| File | Candidate | Date | Source Duration |
|------|-----------|------|----------------|
| [Gustavo Profile](Gustavo_Bezerra_Vibecoder_Profile_Analysis.md) | Gustavo Bezerra de Souza | 2026-02-09 | 1h40min |

### Agent Designs
| File | Description |
|------|-------------|
| [Skills Extraction Agent v1](Vibecoder_Skills_Extraction_Agent_v1.md) | Reusable agent prompt for extracting candidate skills from meetings |

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
- `candidate_skills` - (Proposed) Vibecoder skills extraction from onboarding meetings

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
