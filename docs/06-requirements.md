# 6. Requirements Specification

Priorities (MoSCoW): **M** — must, **S** — should, **C** — could. Every *Must* requirement
has an acceptance criterion that can be checked by a test or a measurement (see
[09-verification-plan.md](09-verification-plan.md)).

Sources of requirements: literature review (L), existing system comparison (E), user
interviews (U — to be confirmed, see [02-relevance-evidence.md](02-relevance-evidence.md)),
team assumption (A).

## 6.1 Actors

- **Guest** — unregistered visitor.
- **Patient** — registered user who uploads photos.
- **Administrator** — manages users and monitors the system.
- **Inference service** and **LLM provider** — external/internal systems.

## 6.2 Functional requirements

| ID | Requirement | Priority | Source | Acceptance criterion |
|---|---|---|---|---|
| FR-1 | The user can register and log in with e-mail and password | M | A | A new account can log in; protected endpoints return 401 without a valid token |
| FR-2 | The user must accept terms and a disclaimer ("screening, not diagnosis") before the first analysis | M | L [13], E | Analysis endpoint returns 403 until consent is recorded; consent date stored |
| FR-3 | The user can upload a JPEG/PNG photo of teeth up to 10 MB | M | E | Valid files are accepted; other formats and > 10 MB are rejected with a clear message |
| FR-4 | The system checks photo quality (resolution, blur, teeth visible) and rejects unusable photos with advice | M | L [1, 9, 12] | On a test set of 50 unusable photos ≥ 80% are rejected; on 50 good photos ≤ 10% are rejected |
| FR-5 | The system detects caries regions and returns boxes with confidence scores | M | L [6] | Response contains a list of findings (box, class, confidence); model meets NFR-7 |
| FR-6 | The result page shows the photo with drawn boxes and a summary ("no signs found" / "signs found, see a dentist") | M | E | For a photo with findings, boxes are drawn at the returned coordinates; summary text matches the findings |
| FR-7 | The system generates a plain-language explanation and recommendation using a multimodal LLM | S | E, L [8] | Explanation is returned in ≤ 15 s in 95% of cases; if the LLM fails, the result is still shown without explanation |
| FR-8 | The LLM explanation must not contradict the detector (no new findings invented) | M | L [8] | Explanation is generated from detector findings passed as facts; test cases with 0 findings never produce "caries found" |
| FR-9 | The user can view the history of their analyses | M | E | The list shows all user analyses with date, thumbnail and number of findings; other users' analyses are not visible |
| FR-10 | The user can delete an analysis and its photo | M | A | After deletion the record and the stored image are removed |
| FR-11 | The user can download a result as PDF to show a dentist | C | U | PDF contains photo with boxes, findings and date |
| FR-12 | The administrator can view users, analyses statistics and failed jobs | S | A | Admin panel lists users and job statuses; non-admins get 403 |
| FR-13 | The interface is available in Russian and English | S | U | All UI strings switch language |
| FR-14 | The user can compare two analyses over time | C | E | Two selected analyses are shown side by side with finding counts |

## 6.3 Non-functional requirements

| ID | Category | Requirement | Priority | Acceptance criterion / measurement |
|---|---|---|---|---|
| NFR-1 | Performance | Model inference per image on a 4-vCPU server | M | p95 ≤ 1 s over 200 test images |
| NFR-2 | Performance | End-to-end time from upload to result with boxes (without LLM text) | M | p95 ≤ 5 s at 10 concurrent users |
| NFR-3 | Scalability | Throughput | S | ≥ 20 analyses per minute on one server without errors |
| NFR-4 | Responsiveness | API endpoints other than upload respond fast while analyses run | M | p95 ≤ 300 ms for history requests during a load test |
| NFR-5 | Availability | The system degrades gracefully | M | If the LLM is unavailable, detection results are still returned (fault-injection test) |
| NFR-6 | Reliability | Failed jobs are retried and reported | S | A job failing in the inference service is retried up to 2 times and then marked as failed with a user message |
| NFR-7 | Detection quality | Model quality on the held-out test split | M | mAP@0.5 ≥ 0.70 and recall ≥ 0.75 on the test split of the open dataset |
| NFR-8 | Security | Passwords hashed, JWT with expiry, HTTPS only | M | Security checklist passed; no plain-text secrets in the repository |
| NFR-9 | Privacy | Photos are private; access only through time-limited links | M | Direct object URL without signature returns 403; signed link expires in ≤ 15 min |
| NFR-10 | Privacy | No personal data or secrets in Git | M | Repository scan (gitleaks) finds no secrets |
| NFR-11 | Usability | Mobile-first UI | M | Key scenarios work at 390 px width without horizontal scroll |
| NFR-12 | Usability | First analysis can be completed without instructions | S | ≥ 4 of 5 test users complete it in ≤ 3 min |
| NFR-13 | Maintainability | Automated tests | M | Backend test coverage ≥ 70%; tests run in CI on every pull request |
| NFR-14 | Portability | One-command deployment | M | `docker compose up` starts all services on a clean machine |
| NFR-15 | Ethics | Disclaimer on every result page | M | Result page contains the "not a diagnosis" notice |

## 6.4 Constraints

- Only open datasets with licences allowing research use are used for training.
- Budget for the LLM API: free tier / ≤ 20 USD for the whole project.
- Model training only on free cloud GPUs (Google Colab / Kaggle).
- The system is not a medical device and must not present diagnoses.

## 6.5 Traceability (requirements → components)

| Requirement | Component |
|---|---|
| FR-1, FR-2, FR-9, FR-10, FR-12 | Backend API (Django) |
| FR-3, FR-4 | Backend API + Inference service (quality check) |
| FR-5, NFR-1, NFR-7 | Inference service (YOLO/ONNX) |
| FR-6, FR-13, NFR-11 | Web application (React) |
| FR-7, FR-8, NFR-5 | Worker + LLM adapter |
| NFR-2, NFR-3, NFR-4, NFR-6 | Worker queue (Celery/Redis) |
| NFR-9 | Object storage (MinIO) + Backend |
