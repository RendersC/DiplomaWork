# 8. UX / Prototype

Clickable prototype: [prototype/index.html](../prototype/index.html) — open the file in a
browser (works on desktop and at mobile width). Screenshots: [prototype/screens](../prototype/screens).

The prototype is a mock-up: data and the photo are illustrative, no backend is called.

## 8.1 Key user scenarios

| # | Scenario | Screens | Requirements |
|---|---|---|---|
| S1 | First visit: register, accept the disclaimer | Login → Consent | FR-1, FR-2 |
| S2 | Upload a photo and get a result | Upload → Processing → Result | FR-3, FR-5, FR-6, FR-7 |
| S3 | Photo is unusable, user retakes it | Upload → Rejected → Upload | FR-4 |
| S4 | Review past analyses | History → Result | FR-9 |
| S5 | Show the result to a dentist | Result → Download PDF | FR-11 |

## 8.2 Design decisions

- **Mobile-first, single column** — most photos are taken with a phone (NFR-11).
- **Photo tips before upload** — literature and the AICaries usability study show that photo
  quality is the main failure point; the tips reduce rejected photos.
- **Processing steps are visible** — analysis takes several seconds (NFR-2); showing steps
  makes waiting understandable.
- **Numbered boxes with confidence** linked to a list of findings — the user sees *where* and
  *how sure* the system is.
- **Explanation separated from findings** — the LLM text explains the detector's findings and
  is visually secondary (FR-8).
- **Disclaimer on consent and on every result** (FR-2, NFR-15).
- **Unusable photo screen gives a concrete reason and advice** instead of a generic error.

## 8.3 Example of the internal API (inference service)

Request: `POST /detect` (multipart, field `image`)

Response:

```json
{
  "quality": { "ok": true, "reason": null, "blur_score": 182.4, "width": 3024, "height": 4032 },
  "model_version": "yolo11s-caries-2026-11",
  "findings": [
    { "box": [412, 655, 96, 88], "class": "caries", "confidence": 0.87 },
    { "box": [1840, 2210, 80, 74], "class": "caries", "confidence": 0.71 }
  ],
  "timings_ms": { "preprocess": 18, "inference": 142, "postprocess": 4 }
}
```

Rejected photo:

```json
{ "quality": { "ok": false, "reason": "blurry", "blur_score": 21.7 }, "findings": [] }
```

## 8.4 Feedback to collect

The prototype is shown in the user interviews (questions Q9–Q11, D3–D5 in
[02-relevance-evidence.md](02-relevance-evidence.md)); changes made after feedback are
recorded in [11-supervisor-feedback.md](11-supervisor-feedback.md) and in this document.
