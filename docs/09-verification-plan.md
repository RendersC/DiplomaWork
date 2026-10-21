# 9. Verification and Evaluation Plan

The project aim is demonstrated by three kinds of evidence: (1) functional tests showing that
every *Must* requirement is met, (2) a detection quality experiment against a baseline,
(3) a performance experiment of the deployed platform.

## 9.1 Testing plan

| Level | Tool | Scope | When |
|---|---|---|---|
| Unit tests | pytest, pytest-django | Serializers, permissions, quality-check rules, LLM response validation (FR-8) | Every pull request (CI) |
| API / integration tests | pytest + Django test client, Docker Compose | Upload → job → findings → history; consent; deletion; access to other users' data | Every pull request |
| Inference service tests | pytest + FastAPI TestClient | Input validation, output schema, model loading, fixed test images | Every pull request |
| Fault injection | pytest with mocked LLM / stopped inference container | NFR-5, NFR-6 | Before each pre-defense |
| End-to-end UI tests | Playwright | Register → consent → upload → result → history at 390 px and 1280 px | Before each pre-defense |
| Load tests | Locust | NFR-2, NFR-3, NFR-4 | Performance experiment (9.4) |
| Security checks | gitleaks, Django `check --deploy`, manual checklist | NFR-8, NFR-9, NFR-10 | Before each pre-defense |
| Usability test | 5 users, tasks + observation | NFR-12 | Before final pre-defense |

Each *Must* requirement in [06-requirements.md](06-requirements.md) is linked to at least one
test; the mapping is kept in `tests/README.md`.

## 9.2 Data split

- Dataset A: Annotated intraoral image dataset (6,313 images) [Lit 6].
- Dataset B: AlphaDent (1,320 images) [Lit 7]; caries classes merged into one class "caries"
  to match dataset A; segmentation masks converted to boxes.
- Split: 70 / 15 / 15 (train / validation / test). Images of the same patient never appear in
  different splits where patient IDs are available.
- The **test split is frozen** before training and used only for the final evaluation.

## 9.3 Detection quality experiment

**Question:** how well does the trained detector find caries compared with a multimodal LLM
used zero-shot?

| Item | Definition |
|---|---|
| System under test | YOLO model exported to ONNX, as served by the inference service |
| Baseline 1 | Gemini 2.5 Flash, zero-shot, asked to return caries boxes as JSON (same prompt for all images) |
| Baseline 2 | Published result on the same dataset: YOLOv8s mAP@0.5 = 0.841 [Lit 6] |
| Test set | Frozen test split of dataset A (≈ 950 images); cross-dataset test on dataset B test split |
| Box-level metrics | Precision, recall, F1 at IoU ≥ 0.5; mAP@0.5; mAP@0.5:0.95 |
| Image-level metrics | Sensitivity, specificity, AUC for "image has caries" |
| Confidence threshold | Chosen on the validation split to maximise F1, then fixed |
| Statistics | 95% confidence intervals by bootstrap (1,000 resamples); McNemar test for image-level detector vs LLM |
| Target | NFR-7: mAP@0.5 ≥ 0.70 and recall ≥ 0.75 on dataset A test split |

Robustness check: metrics on test images with added Gaussian blur and brightness changes, to
estimate behaviour on poorer smartphone photos (following [Lit 9]).

## 9.4 Performance experiment

**Question:** does the platform meet latency and throughput requirements, and how do serving
choices affect them?

| Item | Definition |
|---|---|
| Environment | One server, 4 vCPU, 8 GB RAM, no GPU, Docker Compose; same environment for all runs |
| Workload | Locust: users upload photos from the test split and poll results; 1, 5, 10, 20 concurrent users, 10 min per level, 2 min warm-up excluded |
| Metrics | Inference time per image (p50/p95), end-to-end time to result (p50/p95), throughput (analyses/min), error rate, history endpoint p95 during load, CPU and memory |
| Compared configurations | (a) PyTorch model inside the worker (baseline) vs (b) separate ONNX Runtime service (chosen design); (c) with vs without LLM explanation step |
| Targets | NFR-1 p95 ≤ 1 s inference; NFR-2 p95 ≤ 5 s end-to-end at 10 users; NFR-3 ≥ 20 analyses/min; NFR-4 p95 ≤ 300 ms |

## 9.5 How the aim is demonstrated

| Aim component | Evidence |
|---|---|
| Working platform | Live demo of the main scenario; E2E test report |
| Detection quality | Tables and PR curves from 9.3 vs baselines |
| Performance | Load-test report from 9.4 vs NFR targets |
| Requirements met | Requirement → test mapping with pass/fail status |

## 9.6 Threats to validity

- Open datasets come from limited devices and regions; results may not transfer to all
  smartphone photos — mitigated by cross-dataset testing and robustness checks.
- Labels have expert disagreement [Lit 10]; reported metrics have an upper bound set by label
  quality.
- LLM outputs vary between runs and model versions; the model version and prompt are fixed and
  logged, each image is queried with temperature 0.
