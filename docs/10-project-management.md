# 10. Project Management and Risks

- **Repository:** https://github.com/RendersC/DiplomaWork
- **YouTrack project:** key `DIP` — https://diplomawork.youtrack.cloud/issues/DIP
- **Workflow:** task `DIP-N` → branch `DIP-N-short-name` → commits `DIP-N: …` → pull request
  reviewed by the other team member → merge to `main`. Submissions are tagged
  (`pre-defense-1`, `pre-defense-2`, `final-pre-defense`).

## 10.1 Allocation of responsibilities

| Area | Әділдабек Н.Т. (@RendersC) | Файзрахман Ә.Қ. (@AlikhanF2006) |
|---|---|---|
| Analysis | Proposal, literature review, technology comparison | Relevance evidence and interviews, existing system comparison |
| Design | Requirements, architecture, diagrams | UX prototype, verification plan |
| Implementation | Backend API, worker, inference service, model training, deployment | Web application, CI, test automation |
| Research | Model training and export, performance experiment set-up | Detection quality experiment, LLM baseline, load tests |
| Review | Reviews all pull requests of the other member | Reviews all pull requests of the other member |

## 10.2 Milestones

| Milestone | Date | Deliverables |
|---|---|---|
| M1 Pre-defense 1 | 24 Oct 2026 | Proposal, relevance evidence, literature review, comparisons, requirements, diagrams, prototype, verification plan, YouTrack, tag `pre-defense-1` |
| M2 Data and model baseline | Nov 2026 | Prepared datasets, first trained model with metrics on the validation split |
| M3 Core platform | Dec 2026 | Backend, inference service, web app: upload → result → history working end-to-end |
| M4 Pre-defense 2 | per School schedule | LLM explanation, quality check, deployment, automated tests in CI |
| M5 Experiments | Feb–Mar 2027 | Detection quality experiment, performance experiment, usability test |
| M6 Final pre-defense | per School schedule | Explanatory note, final results, tag `final-pre-defense` |

## 10.3 Tasks for pre-defense 1

| Task | Title | Responsible | Deadline |
|---|---|---|---|
| DIP-1 | Set up repository, README, .gitignore | Әділдабек | 14 Oct |
| DIP-2 | Project proposal and scope | Әділдабек | 14 Oct |
| DIP-3 | Relevance evidence and interview guide | Файзрахман | 15 Oct |
| DIP-4 | Literature review | Әділдабек | 15 Oct |
| DIP-5 | Existing system comparison | Файзрахман | 17 Oct |
| DIP-6 | Technology comparison | Әділдабек | 17 Oct |
| DIP-7 | Requirements specification | Әділдабек | 18 Oct |
| DIP-8 | Architecture description and diagrams | Әділдабек | 19 Oct |
| DIP-9 | UX prototype | Файзрахман | 19 Oct |
| DIP-10 | Verification and evaluation plan | Файзрахман | 21 Oct |
| DIP-11 | Project management, risks, YouTrack link | Әділдабек | 21 Oct |
| DIP-12 | Supervisor feedback log | Әділдабек | 22 Oct |
| DIP-13 | Conduct 3–5 user interviews and record results | Файзрахман | 21 Oct |
| DIP-14 | Tag `pre-defense-1` and submission summary | Әділдабек | 23 Oct |

Full task list with acceptance criteria: [youtrack-tasks.csv](youtrack-tasks.csv) (import file
for YouTrack).

## 10.4 Risks

| # | Risk | Probability | Impact | Mitigation |
|---|---|---|---|---|
| R1 | Open datasets do not represent real smartphone photos; model works worse on users' photos | High | High | Cross-dataset test, augmentation (blur, lighting), photo quality check, honest reporting |
| R2 | Model does not reach the quality target (NFR-7) | Medium | High | Try larger YOLO variants and higher input resolution; combine datasets; report the achieved level against the published baseline |
| R3 | Free GPU limits (Colab/Kaggle) slow down training | Medium | Medium | Small/medium models, train early, checkpoint often, use both platforms |
| R4 | LLM API changes, costs or rate limits | Medium | Medium | Adapter interface, caching, budget cap, system works without explanation (NFR-5) |
| R5 | Users read results as a diagnosis | Medium | High | Disclaimer at consent and on every result, wording checked with a dentist (interviews) |
| R6 | Too few interview participants | Medium | Medium | Ask dental students and clinics in Astana early; online interviews allowed |
| R7 | Uneven workload or a team member unavailable | Low | High | Weekly sync, tasks with owners and deadlines in YouTrack, mutual reviews so both know the code |
| R8 | Leak of secrets or personal data to Git | Low | High | `.gitignore`, `.env.example`, gitleaks in CI, no real patient photos in the repo |
| R9 | Dataset licence restrictions (AlphaDent CC BY-SA, YOLO AGPL) | Low | Medium | Academic, open-source use; attribution kept in docs; licences re-checked before release |

## 10.5 Dependencies

| Dependency | Needed for | Status |
|---|---|---|
| Zenodo caries dataset, AlphaDent | Training and evaluation | Publicly available |
| Google Colab / Kaggle GPU | Model training | Available (free tier) |
| Gemini API key | Explanations and LLM baseline | To be obtained |
| Server (4 vCPU, 8 GB) | Deployment and performance experiment | To be arranged by M3 |
| Interview participants | Relevance evidence | In progress |
| Supervisor reviews | Feedback log, approvals | Weekly |

## 10.6 Next steps after pre-defense 1

1. Download and convert both datasets to a common YOLO format; freeze the test split.
2. Train the first YOLO baseline on Colab and record validation metrics.
3. Create backend and frontend project skeletons with CI.
4. Implement the inference service with ONNX Runtime and the `/detect` contract from doc 08.
