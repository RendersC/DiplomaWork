# CariesScan — Web Platform for Dental Caries Screening from Photographs

Diploma project, School of Software Engineering, Astana IT University (2026–2027).

**Topic:** Development of a Web Platform for Dental Caries Screening from Dental Photographs
with a Neural Network Inference Service and an Investigation into Its Performance and
Detection Quality.

A user uploads a photograph of their teeth. A neural network inference service detects
regions with visual signs of caries and marks them on the image; a multimodal large language
model explains the result in plain language and suggests whether to see a dentist. The
platform keeps the history of analyses. It is a **screening tool, not a diagnostic device**.

## Problem, users, expected outcomes

- **Problem.** Early caries often goes unnoticed until a dental visit, and many people visit a
  dentist only when pain appears. There is no simple, freely available tool that lets a person
  check a photo of their teeth and understand whether a visit is needed.
- **Target users.** Patients (primary), dental clinics that want a pre-visit view of a
  patient's teeth, school oral-health prevention programmes.
- **Expected outcomes.** A working web platform with a separate inference service; a caries
  detection model trained on open labelled datasets; an experimental evaluation of detection
  quality (precision, recall, F1, mAP) against a multimodal LLM baseline and of platform
  performance under load.

Full statement: [docs/01-project-proposal.md](docs/01-project-proposal.md).

## Team

| Member | GitHub | Responsibility |
|---|---|---|
| Әділдабек Нұрасыл Талғатұлы (SE-2408) | [@RendersC](https://github.com/RendersC) | Backend, architecture, inference service, model training, deployment |
| Файзрахман Әлихан Қанатұлы (SE-2408) | [@AlikhanF2006](https://github.com/AlikhanF2006) | Frontend, testing, experimental evaluation |

**Supervisor:** Elvira Aitmukhanbetova, Senior Lecturer, School of Software Engineering.

## Scope and status

**In scope:** photo upload, caries region detection, result visualisation, LLM explanation,
analysis history, user accounts, evaluation of detection quality and performance.
**Out of scope:** clinical diagnosis, X-ray / CBCT analysis, treatment planning, mobile apps.

**Current status (pre-defense 1):** analysis and design stage — proposal, literature review,
system and technology comparisons, requirements, architecture diagrams, prototype and
verification plan. Application code is not implemented yet.

## Selected technologies

| Layer | Technology |
|---|---|
| Detection model | Ultralytics YOLO (v8/11), exported to ONNX |
| Inference service | FastAPI + ONNX Runtime |
| Backend | Python, Django + Django REST Framework, JWT |
| Background jobs | Celery + Redis |
| Storage | PostgreSQL, MinIO (S3-compatible) for photos |
| Frontend | React + TypeScript (Vite) |
| Explanations / LLM baseline | Gemini 2.5 Flash via an adapter |
| Deployment | Docker Compose, Nginx |

Justification: [docs/05-technology-comparison.md](docs/05-technology-comparison.md).

## Links

- YouTrack project: https://diplomawork.youtrack.cloud/issues/DIP
- Documentation: [docs/](docs/)
  - [01 Proposal](docs/01-project-proposal.md) · [02 Relevance evidence](docs/02-relevance-evidence.md) · [03 Literature review](docs/03-literature-review.md)
  - [04 Existing systems](docs/04-existing-systems.md) · [05 Technology comparison](docs/05-technology-comparison.md) · [06 Requirements](docs/06-requirements.md)
  - [07 Architecture](docs/07-architecture.md) · [08 Prototype](docs/08-prototype.md) · [09 Verification plan](docs/09-verification-plan.md)
  - [10 Project management and risks](docs/10-project-management.md) · [11 Supervisor feedback](docs/11-supervisor-feedback.md)
- Diagrams: [diagrams/](diagrams/)
- Prototype: [prototype/](prototype/)

## Repository structure

```
docs/          proposal, literature review, comparisons, requirements, verification plan
diagrams/src   editable diagram sources (PlantUML)
diagrams/export  rendered diagrams (PNG/SVG)
prototype/     UI mockups of key user scenarios
src/backend    Django REST API (planned)
src/frontend   React web application (planned)
src/inference  neural network inference service (planned)
tests/         automated tests and test materials
config/        configuration examples (.env.example)
```

## Setup, execution and testing

Will be added as soon as runnable code is available.

## Workflow

Each work item is a YouTrack task `DIP-N`. Branch: `DIP-N-short-name`, commit message:
`DIP-N: description`. Changes go to `main` only through a pull request reviewed by the other
team member. Each pre-defense submission is tagged (`pre-defense-1`, `pre-defense-2`,
`final-pre-defense`). Secrets, API keys and personal data are never committed.
