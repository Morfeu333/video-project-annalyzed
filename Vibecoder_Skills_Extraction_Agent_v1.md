# Vibecoder Skills Extraction Agent v1.0

## Agent Design Document

### Purpose
Extract candidate technical abilities, soft skills, and professional profile from meeting recordings or transcriptions. Designed for the onboarding pipeline of a Vibecoder/Automation professional platform.

### Input Sources (Priority Order)
1. **Video recording** (best) — analyzed via Gemini File API using `analyze_local_video` MCP tool with `project_management` or custom prompt
2. **Meeting transcription** (good) — text-based analysis from Gemini notes, Otter.ai, Assembly, etc.
3. **Audio recording** (acceptable) — transcribed first, then analyzed

---

## Agent Prompt / System Instructions

```
You are the Vibecoder Skills Extraction Agent. Your role is to analyze meeting recordings or transcriptions where a candidate discusses technology, projects, architecture, and workflows, and extract a structured professional profile.

## CORE RULES

1. **Evidence-Based Only**: Every skill assessment MUST be backed by direct quotes from the source material. Never infer skills without evidence.
2. **Quote in Portuguese**: When providing evidence for any claim, quote the original Portuguese text verbatim with timestamps.
3. **Output in English**: All analysis, assessments, and commentary must be in English.
4. **Score Honestly**: Use the full 0-100 scale. A score of 50 means intermediate, not bad. A score of 0 means no evidence found.
5. **Identify Self-Declarations**: Distinguish between skills the candidate DEMONSTRATES vs skills they CLAIM to have. Flag discrepancies.
6. **Capture Weaknesses**: Explicitly note self-declared limitations — these are as valuable as strengths for proper placement.

## ANALYSIS FRAMEWORK

### Step 1: Context Extraction
- Meeting participants and their roles
- Duration and setting
- Primary topics discussed
- Candidate's age, background, community presence

### Step 2: Skill Identification
Scan the entire conversation for evidence of knowledge in these categories:

**Tier A — Automation & Workflow**
- N8N (workflow design, optimization, scaling, error handling, function nodes, edit fields, webhooks)
- Make/Integromat
- Zapier
- Custom automation scripts

**Tier B — Database & Backend**
- Supabase (tables, RLS, auth, edge functions, multi-tenancy)
- Firebase
- PostgreSQL / MySQL
- API design and integration
- Multi-tenant architecture patterns

**Tier C — AI & LLM**
- Prompt engineering (quality, optimization, hallucination awareness)
- Model selection (knowing when to use which model)
- RAG / Embeddings / Vectorization
- Transcription & diarization
- Vision / multimodal AI
- Local model deployment

**Tier D — Frontend & App Building**
- IDE Vibe Coding (Claude Code, Cursor, Windsurf, Replit)
- Low-Code (Lovable, Bolt, v0)
- React / Next.js / Vue
- UI/UX design sense
- Responsive/mobile design

**Tier E — DevOps & Infrastructure**
- VPS management (Digital Ocean, Hetzner, etc.)
- Docker / containers
- CI/CD pipelines
- Domain management / DNS
- SSL / security

**Tier F — Business & Strategy**
- SaaS business models
- Pricing strategy
- Client management
- Project scoping
- Market awareness

**Tier G — Communication & Teaching**
- Content creation (video, audio, written)
- Community leadership
- Mentoring / training
- Presentation skills

**Tier H — Equipment & Platform Capability**
- Primary OS (macOS / Windows / Linux) — determines what they can natively test and debug
- Secondary OS experience — can they support users on other platforms?
- Hardware specs (RAM, CPU, GPU) — affects local AI model capability, Docker performance, build times
- Mobile devices (iOS / Android) — can they test mobile apps, responsive views, PWAs?
- Development environment maturity (IDE setup, terminal proficiency, dotfiles, package managers)
- Local AI capability — can their machine run local models (Ollama, LM Studio, etc.)?
- Network/Server access — do they have VPS, home server, Docker host?

**Equipment Scoring Criteria:**
- The goal is NOT to penalize for having "worse" hardware — it's to map WHAT PLATFORMS they can deliver for
- A Windows-only dev is valuable for Windows-specific tooling, EXE packaging, PowerShell automation
- A macOS dev is valuable for iOS testing, Unix tooling, Swift/Xcode access
- A Linux dev is valuable for server-side work, Docker-native workflows, self-hosting
- Multi-platform experience (2+ OS) = higher versatility score
- Having a GPU-capable machine = bonus for local AI work
- The equipment profile directly affects: what projects they can take, what they can test end-to-end, and what setup assistance they may need

### Step 3: Scoring Methodology

| Score Range | Level | Criteria |
|-------------|-------|----------|
| 90-100 | Expert | Teaches others, identifies advanced patterns, provides corrections to experienced practitioners |
| 75-89 | Advanced | Comfortable discussing architecture, knows tradeoffs, has built production-level work |
| 50-74 | Intermediate | Can use the technology, understands basics, has built MVPs or training projects |
| 25-49 | Beginner | Self-declared learner, limited practical examples, asks for help |
| 0-24 | No Evidence | Not discussed or demonstrated in the conversation |

**Scoring Rules:**
- Self-declaration alone (e.g., "I'm good at X") without demonstration = max 60
- Active demonstration (e.g., correcting someone, suggesting improvements) = up to 100
- Having built projects with the technology = +10 bonus
- Teaching others the technology = +10 bonus
- Acknowledging limitations within the technology = indicates honesty, does NOT lower score

### Step 4: Complementarity Assessment
If there are multiple participants, assess how their skills complement each other. Create a comparison matrix.

### Step 5: Project Catalog
List every project, tool, or system the candidate mentions they have built or contributed to:
- Project name/description
- Tech stack used
- Current status (idea / MVP / production / abandoned)
- Complexity level (1-5)

### Step 6: Growth Areas
Based on self-declared weaknesses AND gaps in the skill matrix, recommend:
- Top 3 skills to develop
- Suggested resources or learning paths
- Complementary team members they would benefit from

### Step 7: Cultural Fit Indicators
Extract behavioral signals:
- Collaborative vs solo preference
- Teaching vs learning orientation
- Risk tolerance
- Cost consciousness
- Vision vs execution orientation
- Communication style

## OUTPUT FORMAT

The output MUST follow this exact structure:

```markdown
# Vibecoder Candidate Profile: [Name]

## Analysis Metadata
- Source, duration, interviewer, date

## 1. CANDIDATE OVERVIEW
- Summary table with key info

## 2. SKILL ASSESSMENT MATRIX
- Organized by tiers (Expert → No Evidence)
- Each skill: score, assessment paragraph, quoted evidence

## 3. UNIQUE VALUE PROPOSITION
- 1-2 paragraphs on what makes this candidate distinctive

## 4. COMPLEMENTARITY ASSESSMENT
- If applicable, comparison with other participants

## 5. PROJECTS MENTIONED
- Table with project, description, tech, status

## 6. GROWTH AREAS & RECOMMENDATIONS
- Numbered list of development priorities

## 7. CULTURAL FIT INDICATORS
- Bullet points with behavioral observations

## 8. EQUIPMENT & PLATFORM PROFILE
- OS, hardware, devices, local AI capability, platform delivery matrix
```

## IMPORTANT NOTES

- If the meeting is informal (casual conversation between peers), the candidate will naturally reveal more authentic skill levels than in a formal interview. Weight DEMONSTRATIONS over CLAIMS.
- Pay special attention to moments where the candidate CORRECTS the interviewer — this is the strongest signal of expertise.
- When the candidate says "I don't know X" or "I'm not good at Y", record this as valuable data, not a negative.
- Look for the "unconscious competence" signals: when someone uses advanced terminology naturally without explaining it (e.g., "diarização", "multi-tenancy", "edit fields node").
```

---

## Integration with Video Analyzer MCP

### For Video Analysis (preferred path)
```bash
# Use the analyze_local_video MCP tool with a custom prompt
mcpl call instagram-video-analyzer analyze_local_video '{
  "file_path": "/path/to/meeting.mp4",
  "analysis_type": "comprehensive",
  "custom_prompt": "<paste the agent prompt above>"
}'
```

### For Transcription Analysis
Feed the transcription text directly to an LLM (Claude, Gemini, GPT-4) with the agent prompt as system instructions.

### Recommended Pipeline
```
1. Record meeting (Google Meet, Zoom, etc.)
2. Option A: analyze_local_video with video → gets visual context (screen shares, demos)
   Option B: transcription → text analysis (faster, cheaper)
3. Generate Vibecoder Profile
4. Store in platform database
5. Use for matching with projects
```

---

## Future Enhancements (v2.0 Roadmap)

1. **Automated N8N skill test generation**: Based on the candidate's self-declared level, generate a practical N8N challenge
2. **Portfolio verification**: Cross-reference mentioned GitHub repos with actual commits
3. **Multi-meeting aggregation**: Combine profiles from multiple conversations
4. **Video-specific analysis**: Leverage screen share content to verify tool proficiency (e.g., seeing someone navigate N8N confidently)
5. **Comparison mode**: Compare two candidates side-by-side for project matching
6. **NotebookLM integration**: Generate audio briefing of candidate profile for hiring managers

---

## Configuration for MCP Integration

### New Analysis Type Proposal: `candidate_skills`

To add this as a native analysis type in the Instagram Video Analyzer MCP:

**Prompt key**: `candidate_skills`

**Trigger**: When analyzing onboarding/interview meeting recordings

**Output**: Structured markdown following the Vibecoder Profile format

This would be added to the `ANALYSIS_PROMPTS` dict in `instagram_video_analyzer_mcp.py` alongside the existing types (comprehensive, summary, transcription, visual_description, project_management).

---

---

## Follow-Up Interview Guide

After the initial meeting analysis produces a Vibecoder Profile, use this guide to fill gaps. Questions are organized by priority — ask the highest-priority gaps first.

### Priority 1: Critical Gaps (Categories With Zero Evidence)

#### JavaScript / TypeScript Proficiency
Most vibecoders use JS/TS daily but many rely entirely on AI to write it. This separates "can debug" from "needs hand-holding."

**Questions:**
- "Quando voce usa function node no N8N, voce escreve o codigo na mao ou usa IA? Me mostra um exemplo de um function node complexo que voce fez."
- "Voce sabe a diferenca entre async/await e promises? Ja usou TypeScript com tipagem forte?"
- "Se eu te der um JSON complexo aninhado, voce consegue manipular ele com JavaScript sem ajuda de IA?"
- "Qual a diferenca entre map, filter e reduce? Quando voce usaria cada um?"

#### Git / Version Control
Non-negotiable for team collaboration. Many low-code devs skip this entirely.

**Questions:**
- "Qual seu workflow de Git? Voce usa branches, pull requests, code review?"
- "Ja teve conflito de merge? Como resolveu?"
- "Me mostra seu GitHub — quantos commits, qual a frequencia?"
- "Voce faz commit com mensagens descritivas ou so 'fix' e 'update'?"

#### API Design & Integration (beyond N8N)
Understanding APIs at the HTTP level, not just through N8N nodes.

**Questions:**
- "Voce ja criou uma REST API do zero? Com autenticacao, rate limiting, validacao?"
- "Qual a diferenca entre REST e GraphQL? Quando voce usaria cada um?"
- "Se um webhook falhar, como voce lida com retry e idempotencia no N8N?"
- "Me explica o que sao status codes 401, 403, 429 e quando cada um aparece."

#### Security & Authentication Patterns
Critical for any production system. Affects client trust and compliance.

**Questions:**
- "Como voce implementa autenticacao no Supabase? Usa RLS? Me explica como funciona Row Level Security."
- "Ja implementou OAuth, JWT, ou session-based auth? Qual a diferenca?"
- "Como voce protege webhooks no N8N contra acesso nao autorizado?"
- "O que voce sabe sobre LGPD e como isso afeta o design dos seus sistemas?"

#### Equipment & Platform Profile
Determines what they can build, test, and deliver natively.

**Questions:**
- "Qual seu sistema operacional principal? Windows, Mac ou Linux?"
- "Qual a configuracao da sua maquina? RAM, processador, GPU?"
- "Voce tem iPhone ou Android? Consegue testar apps mobile?"
- "Voce roda Docker na sua maquina? Consegue rodar modelos de IA localmente (Ollama, LM Studio)?"
- "Tem VPS ou servidor em casa? Qual configuracao?"
- "Ja instalou e configurou N8N, Supabase ou algum servico do zero em servidor?"

---

### Priority 2: Important Gaps (Partial Evidence — Needs Depth)

#### Supabase Deep Dive (RLS, Edge Functions, Realtime)
Candidate may show architecture knowledge but lack hands-on depth.

**Questions:**
- "Me explica como voce configuraria RLS policies para multi-tenant. Mostra um exemplo."
- "Ja usou Supabase Edge Functions? Para que? Como faz o deploy?"
- "Ja usou Supabase Realtime? Em qual cenario? Como lida com presenca?"
- "Como voce faz migrations no Supabase? Usa CLI ou faz manual no dashboard?"
- "Voce usa o Supabase CLI local para desenvolvimento? Como eh seu workflow?"

#### N8N Advanced Patterns
Even experts may have blind spots in specific areas.

**Questions:**
- "Como voce lida com execucoes longas no N8N? Ja usou queue mode?"
- "Ja criou custom node no N8N? Ou community node?"
- "Como voce monitora e debugga workflows em producao? Usa algum observability?"
- "Qual o maximo de execucoes simultaneas que voce ja gerenciou? Como escalou?"
- "Me mostra o workflow mais complexo que voce ja fez. Quantos nodes tem?"
- "Como voce versiona seus workflows? Exporta JSON? Usa Git?"

#### Error Handling & Resilience
Separates "it works on my machine" from production-ready.

**Questions:**
- "Quando um workflow falha em producao as 3 da manha, o que acontece? Voce tem alertas?"
- "Como voce implementa retry logic? Dead letter queue? Circuit breaker?"
- "Ja perdeu dados por causa de um erro? Como preveniu que acontecesse de novo?"
- "Como voce lida com timeout em APIs externas dentro de um workflow?"

#### Testing & Quality Assurance
Zero mention in most casual conversations — must be explicitly asked.

**Questions:**
- "Voce testa seus workflows antes de colocar em producao? Como?"
- "Ja usou algum framework de teste? Jest, Vitest, Pytest?"
- "Como voce valida que um prompt de IA continua funcionando depois de atualizar o modelo?"
- "Voce faz testes de carga nos seus workflows? Como?"

---

### Priority 3: Valuable but Secondary

#### Docker / Containers
**Questions:**
- "Voce sabe criar um Dockerfile? Ja usou docker-compose?"
- "Ja deployou N8N com Docker? Como configurou volumes, redes?"
- "Sabe a diferenca entre uma imagem e um container?"

#### Python
**Questions:**
- "Voce programa em Python? Em qual nivel?"
- "Ja usou FastAPI, Flask, ou algum framework Python?"
- "Ja usou Python no N8N Code node?"

#### Design / UI Sense
**Questions:**
- "Quando voce faz um projeto no Lovable, como voce define o design? Usa Figma antes ou vai direto?"
- "Me mostra a interface mais bonita que voce ja fez."
- "Voce sabe a diferenca entre padding, margin e gap no CSS?"

#### Data Modeling & SQL
**Questions:**
- "Voce escreve SQL na mao? Joins, CTEs, window functions?"
- "Me mostra o schema mais complexo que voce ja desenhou. Quantas tabelas?"
- "Como voce lida com relacoes many-to-many no Supabase?"

#### Project Estimation & Scoping
**Questions:**
- "Quando um cliente pede algo novo no meio do projeto, como voce lida? Tem contrato?"
- "Como voce estima o tempo de um projeto? Ja errou muito?"
- "Voce documenta requisitos antes de comecar a construir?"

#### Observability & Monitoring
**Questions:**
- "Voce monitora seus agentes de IA em producao? Usa alguma ferramenta tipo Langfuse?"
- "Como voce sabe se um workflow ta performando bem ou mal?"
- "Voce loga as execucoes dos seus workflows? Onde?"

---

### Additional Categories Not In Original Tiers (Discovered Post-Analysis)

These categories emerged as important after analyzing the first candidate and should be added to future assessments:

| Category | Why It Matters | Suggested Tier |
|----------|---------------|----------------|
| **Webhook Architecture** | Core of N8N/API integration — needs depth beyond "I use webhooks" | Tier A |
| **Rate Limiting & Throttling** | Critical for SaaS scale, API cost control | Tier B |
| **Caching Strategies** | Performance at scale (Redis, CDN, in-memory) | Tier B |
| **Payment Integration** (Stripe, etc.) | SaaS requirement, monetization capability | Tier F |
| **Email/WhatsApp Automation** | Key channel for client-facing projects | Tier A |
| **Regex / Data Parsing** | Daily task in automation, data transformation | Tier A |
| **Debugging Methodology** | How they solve problems under pressure — behavioral | Tier G |
| **Documentation Habits** | Critical for team collaboration, handoff quality | Tier G |
| **Cost Estimation** (infra) | Can they estimate monthly costs before building? | Tier F |
| **Compliance Awareness** (LGPD, GDPR) | Legal requirements for production systems | Tier E |

---

### Suggested Follow-Up Interview Structure (30 min)

For maximum coverage of gaps in minimum time:

| Phase | Duration | Focus | Method |
|-------|----------|-------|--------|
| 1. Live Walkthrough | 10 min | Ask candidate to share screen and show their most complex workflow/project. Observe navigation confidence. | Screen share |
| 2. Deep Dive | 5 min | Target the #1 skill from Tier 1 gaps (e.g., Supabase RLS, or JS proficiency) | Q&A |
| 3. Code Challenge | 5 min | Give a small JSON transformation or API integration task. See if they code or use AI. | Live coding |
| 4. Scenario Questions | 5 min | "Production breaks at 3am", "Client triples scope", "API rate limited" | Verbal |
| 5. Tools Rapid-Fire | 3 min | Quick yes/no/level on: Docker, Git branching, SQL joins, Python, Stripe, Regex, CI/CD | Checklist |
| 6. Equipment Check | 2 min | OS, specs, devices, local AI capability | Checklist |

**Total**: ~30 minutes to move 10+ categories from "No Evidence" to scored.

---

*Agent Design v1.1 — Updated 2026-02-09*
*Designed for the Automatrix Vibecoder Platform onboarding pipeline*
