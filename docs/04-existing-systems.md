# 4. Existing System Comparison

## 4.1 Selected systems

Five systems were selected: two research/consumer tools that work with photographs, two
commercial clinical products for radiographs (the main market of dental AI), and a general
multimodal LLM, which is what a patient can actually use today without any dedicated tool.

| System | Type | Input | Source of evidence |
|---|---|---|---|
| **AICaries** | Research smartphone app for parents | Photos of children's teeth | Usability study, PLOS Digital Health, 2022 [A1]; protocol, JMIR Res Protoc, 2021 [A2] |
| **DentalMonitoring** | Commercial medical device software | Photos/scans taken by the patient with a smartphone and accessory | App Store listing [A3]; narrative review of FDA-approved dental AI [A4] |
| **Pearl Second Opinion** | Commercial, FDA-cleared clinical AI | Bitewing, periapical, panoramic X-rays | Vendor product page and FDA clearance announcements [A5] |
| **Diagnocat** | Commercial clinical AI | Intraoral X-rays, panoramic X-rays, CBCT | Vendor product page [A6] |
| **General multimodal LLM (ChatGPT-4o, Gemini)** | General-purpose assistant | Any photo | Khalid et al., BMC Oral Health, 2026 [A7] |

## 4.2 Comparison criteria

| # | Criterion | Why it matters |
|---|---|---|
| C1 | Works with an ordinary photo (no X-ray, no special device) | Needed for home screening |
| C2 | Directly available to patients | Target user is the patient |
| C3 | Detects caries specifically | Core function |
| C4 | Localises findings on the image | User must see *where* the problem is |
| C5 | Explains the result in plain language | Non-expert user |
| C6 | Published, measurable accuracy for caries on photos | Trust in claims |
| C7 | History / tracking over time | Repeated self-checks |
| C8 | Free or low cost | Accessibility |
| C9 | Works in a browser without installation | Low entry barrier |

## 4.3 Comparison table

| Criterion | AICaries | DentalMonitoring | Pearl Second Opinion | Diagnocat | General multimodal LLM | **CariesScan (proposed)** |
|---|---|---|---|---|---|---|
| C1 Ordinary photo | Yes | Partly (app + accessory, clinician-prescribed) | No (X-ray) | No (X-ray/CBCT) | Yes | **Yes** |
| C2 Patient access | Research prototype, not public | Only via clinician | No (dentists only) | No (dentists only) | Yes | **Yes** |
| C3 Caries detection | Yes | No (orthodontic monitoring focus) | Yes | Yes | Not designed for it | **Yes** |
| C4 Localisation | Yes | n/a | Yes (contours) | Yes | Unreliable boxes [A7] | **Yes (boxes + confidence)** |
| C5 Plain-language explanation | Education content | Clinician communicates | For dentists | Structured clinical report | Yes | **Yes (LLM)** |
| C6 Published accuracy on photos | Usability study; photo usability 78.5% [A1] | Not for caries | Radiographs only (vendor-reported) | Radiographs only | F1 68.7–74.3% vs CNN 88.0% [A7] | **Will be measured (doc 09)** |
| C7 History | Not reported | Yes | Patient chart in clinic | Patient records in clinic | No | **Yes** |
| C8 Cost for patient | Free (research) | Through clinic | Clinic subscription | Clinic subscription | Free tier / subscription | **Free** |
| C9 Browser, no install | No (mobile app) | No (mobile app) | Clinic software | Web (for clinics) | Yes | **Yes** |

## 4.4 Conclusions

- Clinical products (Pearl, Diagnocat) are mature and regulated but work with radiographs
  inside clinics; a patient cannot use them at home.
- DentalMonitoring proves that patient-taken photos are accepted in a regulated workflow, but
  it targets orthodontics and requires a clinician.
- AICaries is the closest analogue (photos, caries, parents), but it remains a research
  prototype, is a mobile app, and reports usability rather than detection accuracy.
- A general multimodal LLM is available to everyone, but published evidence shows it is
  significantly less accurate than a trained detector and its localisation is unreliable.

**Need for the proposed solution:** no existing system combines (1) ordinary photos,
(2) direct patient access in a browser, (3) a dedicated caries detector with measured
accuracy, and (4) a plain-language explanation. The project fills this gap.

> Evidence note: statements about commercial products are taken from vendor materials and
> one narrative review; they describe declared functionality, not independently verified
> performance.

## 4.5 Sources

- [A1] Al Jallad N., Ly-Mapes O., Hao P., et al. Artificial intelligence-powered smartphone application, AICaries, improves at-home dental caries screening in children: Moderated and unmoderated usability test. *PLOS Digital Health*, 2022. https://doi.org/10.1371/journal.pdig.0000046
- [A2] Assessing a Smartphone App (AICaries) That Uses Artificial Intelligence to Detect Dental Caries in Children: Protocol. *JMIR Research Protocols*, 2021. https://doi.org/10.2196/32921
- [A3] DentalMonitoring — App Store listing. https://apps.apple.com/us/app/dentalmonitoring/id946052063
- [A4] FDA-Approved AI Solutions in Dental Imaging: A Narrative Review. https://pmc.ncbi.nlm.nih.gov/articles/PMC12775797/
- [A5] Pearl Second Opinion. https://hellopearl.com/products/second-opinion
- [A6] Diagnocat products. https://diagnocat.com/en/products
- [A7] Khalid A. et al. *BMC Oral Health*, 2026. https://doi.org/10.1186/s12903-026-08590-2
