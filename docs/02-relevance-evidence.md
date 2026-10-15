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
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |
| 5 | | | | | |

## 2.5 Summary of findings

To be completed after the interviews: which assumptions were confirmed, which requirements were
changed or re-prioritised as a result.
