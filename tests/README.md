# Tests

Automated tests will be added together with the code. This file keeps the mapping between
*Must* requirements ([docs/06-requirements.md](../docs/06-requirements.md)) and the tests that
verify them ([docs/09-verification-plan.md](../docs/09-verification-plan.md)).

| Requirement | Planned test | Type | Status |
|---|---|---|---|
| FR-1 | Register, log in, 401 without token | API | planned |
| FR-2 | Analysis returns 403 before consent | API | planned |
| FR-3 | Reject unsupported format and > 10 MB | API | planned |
| FR-4 | Quality check on 50 unusable / 50 good photos | Inference | planned |
| FR-5 | `/detect` returns findings schema on fixed images | Inference | planned |
| FR-6 | Boxes drawn at returned coordinates | E2E (Playwright) | planned |
| FR-8 | Explanation never reports caries when there are 0 findings | Unit | planned |
| FR-9 | History shows only own analyses | API | planned |
| FR-10 | Delete removes record and stored image | API | planned |
| NFR-1, NFR-2, NFR-4 | Latency under load | Load (Locust) | planned |
| NFR-5 | Result returned when LLM is down | Fault injection | planned |
| NFR-7 | mAP@0.5 and recall on frozen test split | Experiment | planned |
| NFR-9 | Unsigned image URL returns 403 | API | planned |
| NFR-10 | No secrets in repository | gitleaks (CI) | planned |
| NFR-11 | Key scenarios at 390 px | E2E (Playwright) | planned |
| NFR-14 | `docker compose up` on a clean machine | Manual | planned |
| NFR-15 | Disclaimer on result page | E2E | planned |
