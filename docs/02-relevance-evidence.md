# 2. Evidence of Relevance

This document separates **verified findings** (from published data) from **assumptions**
that are checked with potential users. Interview results are added as they are collected.

## 2.1 Analysis of available data (verified)

| Finding | Evidence | Implication for requirements |
|---|---|---|
| Untreated caries is the leading oral condition; oral diseases affect close to 3.5 billion people | WHO Global Oral Health Status Report 2022; Jain et al. 2023 [Lit 15, 16] | The problem is large and common |
| Deep learning detects caries on ordinary photos, but smartphone photos give lower F1 (42.8–80%) than professional cameras | Moharrami et al. 2023 [Lit 1] | FR-4 photo quality check; honest presentation of uncertainty |
| Parents could take usable photos for AI in 78.5% of cases; users wanted guidance on taking photos | AICaries usability study [Exist A1] | FR-4 must give advice on how to retake the photo |
| Users valued self-monitoring but worried about accuracy without professional guidance | AICaries usability study [Exist A1] | FR-2 disclaimer, FR-11 PDF for the dentist |
| General LLMs are significantly less accurate than a trained CNN for caries | Khalid et al. 2026 [Lit 8] | Detector for detection, LLM only for explanation (FR-7, FR-8) |
| Open labelled data exists (7,600+ photos) | [Lit 6, 7] | Training without collecting patient photos |

## 2.2 Assumptions to verify

| ID | Assumption | How it is checked |
|---|---|---|
| A1 | People do not visit a dentist until pain appears | Patient interviews, Q1–Q3 |
| A2 | Patients would upload a photo of their teeth to a web service | Patient interviews, Q6–Q8 |
| A3 | A result with marked regions + plain explanation is understandable | Patient interviews with prototype, Q9–Q11 |
| A4 | Dentists find a patient's photo with marked regions useful before a visit | Dentist interview, D1–D5 |
| A5 | Users accept the "screening, not diagnosis" limitation | Q12, D6 |

## 2.3 Interview plan

- **Participants:** 3–5 people in total: 1–2 dentists (or dental students of senior years)
  and 2–3 potential patients (students, parents).
- **Format:** 15–20 min, in person or online, notes taken with consent; no personal health
  data recorded.
- **Prototype:** the clickable prototype ([prototype/index.html](../prototype/index.html)) is
  shown during questions Q9–Q11 and D3–D5.

### Questions for potential patients

1. When did you last visit a dentist? What was the reason?
2. Do you usually go for check-ups or only when something hurts?
3. What stops you from visiting a dentist more often?
4. Have you ever noticed dark spots or holes on your teeth? What did you do?
5. Have you ever searched online or asked an AI assistant about your teeth?
6. Would you take a photo of your teeth and upload it to a website to check them? Why / why not?
7. What would make you trust such a service? What would make you distrust it?
8. Would you be concerned about privacy of the photo? What guarantees would you need?
9. *(prototype)* Is the result screen clear? What do the boxes mean to you?
10. *(prototype)* Is the explanation text helpful? What is missing?
11. *(prototype)* What would you do after seeing "signs of caries found"?
12. The service says "this is not a diagnosis". Is it still useful for you?

### Questions for dentists

1. How often do patients come too late with caries that could have been treated earlier?
2. Would a patient's photo with marked suspicious regions be useful before a visit?
3. *(prototype)* Is the way findings are shown clinically reasonable? What is misleading?
4. *(prototype)* Which photo angles are needed to see most caries?
5. What false result is worse for a screening tool: missing caries or a false alarm?
6. What wording should the service use so that patients do not treat it as a diagnosis?

## 2.4 Interview log

| # | Date | Role | Format | Key findings | Confirms / refutes |
|---|---|---|---|---|---|
| 1 | 19.10.2026 | Dentist | In person, with prototype demonstration, 6 questions (D1–D6) | Patients with advanced caries come almost every day; most visit only when pain appears. A pre-visit photo with marked regions would be convenient but never replaces examination and X-ray. Patients may misread an automatic result. One photo is not enough: a basic protocol needs a smile photo, a frontal photo with retractors and occlusal photos of both arches. Missing caries is worse than a false alarm. The result must be called an assumption, not a diagnosis. | Confirms A1, A4 (partly), A5 |
| 2 | 21.10.2026 | Potential patient (P1), 18 | Online, messenger, 6 questions | Visits only when pain appears; would try the service if no registration or phone number is needed; trusts it more if dentists took part; wants the photo deleted right away; a result is a reason to book a visit | Confirms A1, A2 (with conditions), A5 |
| 3 | 21.10.2026 | Potential patient (P2), 21 | Online, messenger, 6 questions | Has yearly check-ups; unsure how to photograph back teeth; wants to know who built it and whether dentists validated it; wants to delete photos; wants a personal explanation, not a generic "see a dentist" | Does not confirm A1 for this participant; A2 partly; confirms A5 |
| 4 | 21.10.2026 | Potential patient (P3), 20 | Online, messenger, 6 questions | Visits only when pain appears (fear, cost); would upload to check at home first; distrusts "everything is bad" for any photo and clinic ads; a "nothing found" result would make them postpone the dentist | Confirms A1, A2, A5; new risk: false reassurance |
| 5 | 21.10.2026 | Potential patient (P4), 23 | Online, messenger, 6 questions | Goes only with a specific problem; would give up if an app, account and questionnaire are required; wants examples validated by a dentist and honesty about bad photos; wants opt-in consent for using photos for training; wants to see which tooth to check | Confirms A1, A2 (if quick), A5 |

Notes: [interviews/01-dentist-2026-10-19.md](interviews/01-dentist-2026-10-19.md),
[interviews/02-patients-2026-10-21.md](interviews/02-patients-2026-10-21.md).

## 2.5 Summary of findings

Status: 5 interviews conducted: 1 dentist (in person, with the prototype) and 4 potential
patients aged 18–23 (online, without the prototype). The sample is small and consists of young
people from the team's circle, so the results indicate directions, not statistics.

| Assumption | Result | Evidence |
|---|---|---|
| A1 People do not visit a dentist until pain appears | Confirmed: the dentist sees such patients almost daily; 3 of 4 patients go only with a problem (forgetting, fear, cost, lack of time) | D1; P1, P3, P4 (P2 has yearly check-ups) |
| A2 Patients would upload a photo of their teeth | Confirmed with conditions: 4 of 4 would try it, but only if it is quick and needs no app, registration or phone number | P1–P4 (Q6) |
| A3 Result with marked regions and explanation is understandable | Not checked with patients (prototype not shown). Patients want a personal explanation and to see which tooth to check; the dentist warns that results may be misread | D3; P2, P4 |
| A4 Dentists find a marked photo useful before a visit | Partly confirmed: "convenient" for preparation, but clinical examination and X-ray remain necessary | D2 |
| A5 "Screening, not diagnosis" is acceptable | Confirmed by all 5 participants; the dentist recommends calling the result "an assumption" | D6; P1–P4 (Q12) |

New findings not covered by the assumptions:

- **False reassurance.** A "nothing found" result may make a user postpone the dentist (P3),
  while the dentist considers missed caries worse than a false alarm (D5).
- **Trust** comes from dentist involvement and validated examples (P1, P2, P4) and is lost
  through ads, sales and "everything is bad" for every photo (P1, P3).
- **Privacy.** Users want to delete photos (P1, P2) and to decide themselves whether photos
  may be used for model training (P4).
- **Taking photos.** Back teeth are hard to photograph (P2); one photo cannot show all surfaces (D4).

### Proposed changes to requirements (to be decided with the supervisor)

| Finding | Proposed change |
|---|---|
| One photo cannot show all surfaces; back teeth are hard to photograph (D4, P2) | Extend photo tips (FR-4, upload screen) with example photos: frontal and, where possible, upper and lower occlusal views; consider several photos per analysis (new Could requirement); state on the result page which areas were not visible. |
| Missing caries is worse than a false alarm; "nothing found" may delay a visit (D5, P3) | Choose the detection threshold in favour of recall (NFR-7). Word a negative result as "no visible signs on this photo — this does not replace a regular check-up" and always recommend a yearly visit. |
| Patients may misread the result and want a personal explanation (D3, D6, P2, P4) | Keep FR-2 and NFR-15; FR-6 numbers findings by position on the photo so the user knows which tooth to show a dentist; the FR-7 explanation refers to the user's own findings. |
| Registration is a barrier (P1, P4) | Consider a guest analysis without an account (result not saved), with registration only for history (FR-1, FR-9); no phone number required. |
| Privacy expectations (P1, P2, P4) | Keep FR-10 (delete) and NFR-9; add an explicit opt-in, off by default, for using photos for training, and state retention on the consent screen. |
| Trust (P1–P4) | Show on the site how the model was evaluated and a validated example; no ads or clinic promotion. |
| Dentists rely on visual examination and X-ray (D2, D6) | Keep X-ray analysis out of scope; position the PDF report (FR-11) as pre-visit information, not as a diagnostic document. |
