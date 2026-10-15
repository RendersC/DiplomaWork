# 3. Literature Review

## 3.1 Search strategy

Sources were searched in Crossref, OpenAlex, PubMed/PMC and Google Scholar using the queries
*caries detection intraoral photographs deep learning*, *dental caries smartphone images*,
*YOLO dental caries*, *multimodal large language model caries*, *AI in dentistry*. Inclusion
criteria: peer-reviewed work on caries detection from **photographs** (not radiographs),
datasets of intraoral photographs, and works that define the methods used in this project.
Most sources are from 2021–2026 because the field changes quickly; three earlier works are
retained as foundational: YOLO (2016), Schwendicke et al. (2020) and Zhang et al. (2020). Every DOI below was checked against
Crossref/OpenAlex.

Total: **17 sources**, of which **15 are peer-reviewed scientific publications** (12 of them
from 2021–2026) and 2 are an official report and technical documentation.

## 3.2 Critical review

### What is already known

**Photographs are a viable input for caries detection.** Kühnisch et al. [3] showed that a CNN
trained with transfer learning on 2,417 standardised intraoral photographs detects caries in
92.5% of cases against an expert reference. Zhang et al. [2] reached an image-level AUC of
85.7% on 3,932 photographs taken with *consumer* cameras, but box-level sensitivity was only
64.6%, i.e. the model finds that "something is wrong" in a photo more reliably than it
localises the lesion. This gap between image-level and lesion-level quality is a recurring
finding and is one of the reasons this project reports both levels.

**Smartphone images are harder.** The systematic review by Moharrami et al. [1] (19 studies)
reports F1 of 68.3–95.4% for professional cameras but only 42.8–80% for smartphone cameras,
and notes that metrics are reported inconsistently. Krothapalli & Kapalavayi [12] reach the
same conclusion: saliva, lighting and camera angle degrade quality, and early (non-cavitated)
lesions remain difficult. Thanh et al. [4] confirm this numerically: YOLOv3 reached 87.4%
sensitivity for cavitated lesions on iPhone photos, but only 36.9% for visually non-cavitated
lesions. Kang & Kim [10] show that self-taken smartphone images are feasible for a screening
aid (sensitivity ≥ 86%) but that inter-examiner agreement between dentists was only
κ = 0.36–0.41, so the "ground truth" itself is uncertain.

**One-stage detectors dominate recent work.** YOLO [14] frames detection as a single
regression problem and is fast enough for interactive use. Recent caries studies use YOLO
variants: YOLOX in ToothNet (F1 0.81 at tooth level, better than a dentist with one year of
experience) [5]; YOLOv8s as the best of five models (mAP@0.5 = 0.841) on the open dataset of
6,313 images [6]; YOLOv12x vs YOLOv8x with ICDAS classes (mAP@0.5 0.762 vs 0.736) [9];
WaveViT-YOLO on the same open dataset (mAP@0.5 0.814) [11]. Notably [11] reports that its
pre-processing stage adds ~235 ms per image on CPU, making the pipeline not real-time —
evidence that **architecture and serving decisions matter, not only accuracy**.

**Open datasets now exist.** Until 2025 almost all work used private clinic data. The dataset
of Faizan Ahmed et al. [6] (6,313 images, CC BY 4.0) and AlphaDent [7] (1,320 DSLR
photographs, instance segmentation, 9 classes, open licence) make reproducible work possible
without collecting patient photos. Limitations: [6] was captured with a single mobile device
in one region and age group (10–24 years); [7] uses DSLR images, which differ from smartphone
photos.

**Multimodal LLMs are not a replacement for a trained detector.** Khalid et al. [8] compared
ChatGPT-4o and Gemini 2.5 Flash with a YOLOv5s-based CNN on intraoral images: the CNN reached
F1 88.0%, Gemini 74.3%, ChatGPT-4o 68.7%, and the difference was significant. The study used
only 22 images, so the effect size is uncertain, but the direction is clear. This motivates
the project design: the detector finds lesions, the LLM only explains the result.

**Context and constraints.** The WHO Global Oral Health Status Report [15, 16] identifies
untreated caries as the leading oral condition worldwide (oral diseases affect close to
3.5 billion people). Schwendicke et al. [13] list the reasons AI has not reached routine
dental practice: limited data, weak methodological standards, unclear practical value, and
ethics/responsibility. The project addresses the first two by using open data and
standardised metrics, and the last one by positioning the system as screening, not
diagnosis.

### Limitations the project takes into account

| Limitation found in the literature | How the project responds |
|---|---|
| Lesion-level performance is much lower than image-level [2] | Report both image-level and box-level metrics |
| Smartphone images give lower F1 than professional cameras [1, 12] | Photo quality check before inference; report results per image source |
| Early lesions are poorly detected [4, 12] | Present results as "signs of caries", not as staging; no diagnostic claims |
| Inconsistent metrics across studies [1] | Fixed metric set: precision, recall, F1, mAP@0.5, mAP@0.5:0.95 |
| Low inter-examiner agreement [10] | Use dentist-verified open labels; discuss label uncertainty in results |
| Heavy pipelines are not real-time [11] | Measure end-to-end latency; export the model to ONNX; keep pre-processing light |
| LLMs underperform trained CNNs [8] | LLM used for explanation; compared as a baseline, not used for detection |
| Single-device, single-region datasets [6] | Combine two datasets [6, 7]; report cross-dataset results |

### Research gap

Existing studies optimise model accuracy on fixed test sets. They rarely deliver the model as
a working software system and do not report system-level properties: end-to-end latency,
throughput under load, failure behaviour when the photo is unusable, or the cost of an LLM
alternative. This project combines a trained detector with a multimodal LLM in one platform
and evaluates **both detection quality and system performance**, which matches the Software
Engineering focus of the diploma.

## 3.3 Analysis table

| # | Source | Year | Data | Method | Key result | Relevance / limitation |
|---|---|---|---|---|---|---|
| 1 | Moharrami et al., *Oral Diseases* | 2023 | 19 studies | Systematic review (QUADAS-2) | F1 42.8–80% smartphone vs 68.3–95.4% professional | Sets expectations; metrics inconsistent |
| 2 | Zhang et al., *Oral Diseases* | 2020 | 3,932 consumer-camera photos | SSD-based ConvNet, hard negative mining | AUC 85.7%; box sensitivity 64.6% | Image vs lesion level gap |
| 3 | Kühnisch et al., *J Dent Res* | 2021 | 2,417 photos | CNN, transfer learning | 92.5% correct detection | Standardised photos, not smartphone |
| 4 | Thanh et al., *Applied Sciences* | 2022 | 1,902 iPhone photos | Faster R-CNN, YOLOv3, RetinaNet, SSD | YOLOv3 sens. 87.4% cavitated, 36.9% non-cavitated | Smartphone; early lesions hard |
| 5 | Xiong et al., *BMC Oral Health* | 2024 | 1,020 photos | ToothNet (YOLOX) | Tooth-level F1 0.81 > 1-year dentist | Detector beats junior clinician |
| 6 | Faizan Ahmed et al., *Scientific Data* | 2025 | 6,313 photos (open, CC BY 4.0) | YOLOv5/8/11, SSD, Faster R-CNN | YOLOv8s mAP@0.5 0.841 | **Training data**; single device |
| 7 | Sosnin et al., *Computer Optics* (AlphaDent) | 2025 | 1,320 DSLR photos (open) | Instance segmentation | High-quality predictions; code open | **Second dataset**; DSLR domain |
| 8 | Khalid et al., *BMC Oral Health* | 2026 | 22 images | GPT-4o, Gemini 2.5 Flash vs YOLOv5s | CNN F1 88.0% vs Gemini 74.3% vs GPT-4o 68.7% | **LLM baseline**; small sample |
| 9 | Tao et al., *BMC Oral Health* | 2026 | 1,606 photos | YOLOv12x vs YOLOv8x, ICDAS | mAP@0.5 0.762 vs 0.736; blur robustness | Newer YOLO helps; blur matters |
| 10 | Kang & Kim, *Frontiers in Oral Health* | 2026 | 70 adults, self-taken photos | YOLOv6-L6 | Sens. ≥ 86%, spec. > 97% | Self-screening feasible; κ 0.36–0.41 |
| 11 | Neji et al., *Applied Sciences* | 2026 | Open dataset [6] | WaveViT-YOLO (YOLOv9m) | mAP@0.5 0.814; 262 ms/image end-to-end | Latency of pre-processing |
| 12 | Krothapalli & Kapalavayi, *J Global Oral Health* | 2025 | Review | Systematic review, smartphone | Good for cavities, weak for early lesions | Screening use-case |
| 13 | Schwendicke et al., *J Dent Res* | 2020 | — | Narrative review | Barriers: data, rigour, value, ethics | Foundational context |
| 14 | Redmon et al., *CVPR* | 2016 | — | YOLO | One-stage real-time detection | Foundational method |
| 15 | Jain et al., *Oral Diseases* | 2023 | WHO data | Report analysis | ~3.5 bn people with oral disease | Problem relevance |
| 16 | WHO, Global Oral Health Status Report | 2022 | Global data | Official report | Untreated caries leading condition | Problem relevance |
| 17 | Ultralytics YOLO and ONNX Runtime documentation | 2026 | — | Technical docs | Training, export, inference APIs | Implementation reference |

## 3.4 References

1. Moharrami M., Farmer J., Singhal S., et al. Detecting dental caries on oral photographs using artificial intelligence: A systematic review. *Oral Diseases*, 2023. https://doi.org/10.1111/odi.14659
2. Zhang X., Liang Y., Li W., et al. Development and evaluation of deep learning for screening dental caries from oral photographs. *Oral Diseases*, 2020. https://doi.org/10.1111/odi.13735
3. Kühnisch J., Meyer O., Hesenius M., Hickel R., Gruhn V. Caries Detection on Intraoral Images Using Artificial Intelligence. *Journal of Dental Research*, 2021. https://doi.org/10.1177/00220345211032524
4. Thanh M.T.G., Van Toan N., Ngoc V.T.N., et al. Deep Learning Application in Dental Caries Detection Using Intraoral Photos Taken by Smartphones. *Applied Sciences*, 12(11):5504, 2022. https://doi.org/10.3390/app12115504
5. Xiong Y., Zhang H., Zhou S., et al. Simultaneous detection of dental caries and fissure sealant in intraoral photos by deep learning: a pilot study. *BMC Oral Health*, 2024. https://doi.org/10.1186/s12903-024-04254-1
6. Faizan Ahmed S.M., Ghori M.H., Khalid A., et al. Annotated intraoral image dataset for dental caries detection. *Scientific Data*, 2025. https://doi.org/10.1038/s41597-025-05647-9 (dataset: https://doi.org/10.5281/zenodo.14827784)
7. Sosnin E., Vasil'ev Y.L., Solovyev R.A., et al. AlphaDent: A dataset for automated tooth pathology detection. *Computer Optics*, 2025. https://doi.org/10.18287/coj1802 (dataset: https://github.com/ZFTurbo/AlphaDent)
8. Khalid A., Nooruddin A., Naved N., et al. Assessing diagnostic performance of multimodal LLMs and a custom convolutional neural network in tooth-level caries detection and localization. *BMC Oral Health*, 2026. https://doi.org/10.1186/s12903-026-08590-2
9. Tao R., Zhang M., Sun Y., et al. Deep learning-based classification of dental caries from intraoral photographs using ICDAS criteria. *BMC Oral Health*, 2026. https://doi.org/10.1186/s12903-026-09688-3
10. Kang J.-Y., Kim H.-K. Feasibility of multi-class dental caries detection using deep learning–based smartphone images: a pilot prospective study. *Frontiers in Oral Health*, 2026. https://doi.org/10.3389/froh.2026.1805448
11. Neji I., Filali I., Ejbali R. WaveViT-YOLO: A Hybrid Architecture for Dental Caries Detection in Intraoral Photographs. *Applied Sciences*, 2026. https://doi.org/10.3390/app16168257
12. Krothapalli N., Kapalavayi N.C.C. Deep learning in dental diagnostics: Caries detection through smartphone photographs – A systematic review. *Journal of Global Oral Health*, 2025. https://doi.org/10.25259/jgoh_30_2025
13. Schwendicke F., Samek W., Krois J. Artificial Intelligence in Dentistry: Chances and Challenges. *Journal of Dental Research*, 99(7), 2020. https://doi.org/10.1177/0022034520915714
14. Redmon J., Divvala S., Girshick R., Farhadi A. You Only Look Once: Unified, Real-Time Object Detection. *CVPR*, 2016. https://doi.org/10.1109/cvpr.2016.91
15. Jain N., Dutt U., Radenkov I., Jain S. WHO's global oral health status report 2022: Actions, discussion and implementation. *Oral Diseases*, 2023. https://doi.org/10.1111/odi.14516
16. World Health Organization. *Global oral health status report: towards universal health coverage for oral health by 2030*. Geneva: WHO, 2022.
17. Ultralytics YOLO documentation (https://docs.ultralytics.com) and ONNX Runtime documentation (https://onnxruntime.ai/docs), accessed October 2026.
