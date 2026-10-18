# 5. Technology Comparison

Each decision is evaluated against requirements from [06-requirements.md](06-requirements.md).
Scores: 3 = good, 2 = acceptable, 1 = weak.

## 5.1 Detection model

Requirements: NFR-1 (inference ≤ 1 s per image on CPU), FR-4 (boxes with confidence),
training on a free cloud GPU, open licence of training tooling.

| Criterion | **YOLOv8/YOLO11 (Ultralytics)** | Faster R-CNN (torchvision) | Multimodal LLM zero-shot (Gemini) |
|---|---|---|---|
| Accuracy on the open caries dataset | 3 — YOLOv8s mAP@0.5 0.841, best of 5 models [Lit 6] | 2 — lower than YOLOv8s in the same study [Lit 6] | 1 — F1 74.3% vs 88.0% for CNN [Lit 8] |
| CPU inference speed | 3 — one-stage, small variants | 1 — two-stage, slow | 1 — network round-trip, seconds |
| Training effort on Colab/Kaggle | 3 — single CLI command, YOLO format available in dataset | 2 — custom training loop | 3 — no training |
| Export for serving (ONNX) | 3 — built-in export | 2 — possible, more manual | n/a |
| Cost per image | 3 — free after training | 3 | 1 — paid API |
| Licence | 2 — AGPL-3.0 (acceptable for an open academic project) | 3 — BSD | 1 — proprietary API |
| **Total** | **17** | 13 | 7 |

**Decision:** YOLO (Ultralytics), small/medium variant. The multimodal LLM is kept as the
**baseline for comparison** and for explanations, not for detection.

## 5.2 Model serving (inference service)

Requirements: NFR-1 latency, NFR-4 the web backend must not block during inference,
independent deployment of the model.

| Criterion | **FastAPI + ONNX Runtime** | Inference inside Django process (PyTorch) | NVIDIA Triton Inference Server |
|---|---|---|---|
| CPU latency | 3 — ONNX Runtime optimised for CPU | 2 — PyTorch eager mode | 3 |
| Isolation from web backend | 3 — separate container | 1 — model memory and CPU load inside web workers | 3 |
| Operational complexity | 3 — small Python service | 3 — nothing new | 1 — heavy, GPU-oriented |
| Independent model updates | 3 | 1 — redeploy backend | 3 |
| Team experience | 3 — Python | 3 | 1 |
| **Total** | **15** | 10 | 11 |

**Decision:** separate inference service on FastAPI with ONNX Runtime.

## 5.3 Backend platform

Requirements: FR-1 accounts, FR-6 history, admin tools, REST API, fast delivery by a team
of two.

| Criterion | **Django + Django REST Framework** | FastAPI (whole backend) | Node.js (NestJS) |
|---|---|---|---|
| Built-in auth, admin panel, ORM, migrations | 3 | 1 — assembled from libraries | 2 |
| Same language as ML code | 3 — Python | 3 — Python | 1 — TypeScript |
| Team experience | 3 — production experience | 2 | 1 |
| Async performance | 2 — sync by default, long work moved to a queue | 3 | 3 |
| **Total** | **11** | 9 | 7 |

**Decision:** Django + DRF for the main backend.

## 5.4 Component interaction

Requirements: NFR-1 latency, NFR-4 non-blocking API, NFR-6 resilience when inference or LLM is
slow or unavailable.

| Criterion | Synchronous REST call | **Task queue (Celery + Redis) + REST to inference** | Message broker (RabbitMQ / Kafka) |
|---|---|---|---|
| API stays responsive under load | 1 | 3 | 3 |
| Retries and timeouts for LLM/inference | 1 | 3 | 3 |
| Infrastructure complexity | 3 | 2 — Redis also used as cache | 1 |
| Status tracking for the UI | 1 | 3 — job status polling | 2 |
| **Total** | 6 | **11** | 9 |

**Decision:** analysis job is created by the API, processed by a Celery worker, which calls
the inference service over REST and then the LLM; the UI polls job status.

## 5.5 Data storage

| Criterion | **PostgreSQL + S3-compatible object storage (MinIO)** | PostgreSQL with images as BLOBs | MongoDB + GridFS |
|---|---|---|---|
| Relational data (users, analyses, findings) | 3 | 3 | 2 |
| Large image storage | 3 | 1 — bloats DB, slow backups | 2 |
| Time-limited access to private images (pre-signed URLs) | 3 | 1 | 1 |
| Team experience | 3 | 3 | 1 |
| **Total** | **12** | 8 | 6 |

**Decision:** PostgreSQL for structured data, MinIO for photographs.

## 5.6 Frontend

| Criterion | **React + TypeScript (Vite)** | Vue 3 | Server-rendered Django templates |
|---|---|---|---|
| Interactive drawing of boxes on photos | 3 | 3 | 1 |
| Ecosystem and component libraries | 3 | 2 | 1 |
| Team experience | 3 | 1 | 2 |
| **Total** | **9** | 6 | 4 |

**Decision:** React + TypeScript.

## 5.7 Explanation LLM

| Criterion | **Gemini 2.5 Flash** | GPT-4o | Local open model (Qwen2.5-VL 7B) |
|---|---|---|---|
| Image input + structured JSON output | 3 | 3 | 2 |
| Cost / free tier | 3 | 2 | 3 (but needs GPU) |
| Measured on caries images | 3 — F1 74.3%, better than GPT-4o [Lit 8] | 2 — F1 68.7% [Lit 8] | 1 — no data |
| Infrastructure | 3 — API | 3 — API | 1 — GPU server |
| **Total** | **12** | 10 | 7 |

**Decision:** Gemini 2.5 Flash behind an adapter interface, so the provider can be replaced.

## 5.8 Summary

| Decision | Choice |
|---|---|
| Detection model | Ultralytics YOLO (v8/11), exported to ONNX |
| Serving | FastAPI + ONNX Runtime, separate container |
| Backend | Django + DRF |
| Interaction | REST + Celery/Redis job queue |
| Storage | PostgreSQL + MinIO |
| Frontend | React + TypeScript |
| LLM | Gemini 2.5 Flash via adapter |
| Deployment | Docker Compose, Nginx |
