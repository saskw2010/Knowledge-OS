# Radiology Pilot Discovery v0.1 â€” Handover

## Outcome

Created a bounded, independent Radiology Discovery artifact for Knowledge-OS:

- `data/radiology-pilot-discovery-v0.1.json`
- `docs/RADIOLOGY-PILOT-HANDOVER-v0.1.md`

This is discovery-only. It does not modify Mathematics/Cybersecurity, schemas, training, models, adapters, or engine code. No large dataset was generated.

## Domain Map

The pilot contains 8 proposed disciplines and 35 proposed subdisciplines:

1. Foundations of Imaging
2. Imaging Modalities
3. Anatomic and Organ-System Imaging
4. Interventional and Image-Guided Radiology
5. Oncologic and Hybrid Imaging
6. Radiation Safety and Quality
7. Radiology Informatics and Operations
8. Radiology Research and Education

Each subdiscipline has one bounded atomicity candidate, for 35 candidates total. Every candidate is explicitly `candidate_atomic`; none is promoted to `atomic`.

## Safety boundary

The artifact separates:

- `educational_general`: definitions, modality principles, terminology, workflow, high-level safety, quality, informatics, research, and communication concepts.
- `clinical_guidance_sensitive`: topics that can drift into exam selection, protocoling, image interpretation, staging, dose/contrast decisions, procedures, or treatment. These are reference-only and review-gated.

Hard limits are explicit: no diagnosis, no treatment recommendation, no patient-specific medical advice, no patient images or records, and no claim that the map replaces a radiologist, referring clinician, medical physicist, local policy, or current professional guidance.

## Confidence and review

Confidence values are prioritization signals, not probabilities or clinical certainty. The semantic state is `unreviewed` throughout because this is an initial hypothesis. A future `machine_validated` state may only mean that structural checks passed; it must not be read as clinical validation.

The artifact records 10 open review items:

- classification overlap between modalities, organ systems, nuclear medicine, and oncology;
- atomicity split/merge decisions;
- safety classification for MRI, contrast, radiopharmaceuticals, pediatrics, and radiation;
- source versions, licenses, and permitted use;
- Arabic terminology and translation disagreement;
- patient-facing communication limits;
- non-reproduction of restricted ACR guidance;
- DICOM edition and future crosswalk scope;
- independent evaluation and bias controls for imaging AI.

Required reviewers include a radiologist or nuclear-medicine physician, radiology educator, medical physicist, radiology informatics specialist, Arabic terminology reviewer, provenance/legal reviewer, and clinical safety reviewer as applicable.

## Provenance posture

The JSON records source pointers to RSNA RadLex, ACR Appropriateness Criteria, ACR Practice Parameters and Technical Standards, the WHO/IAEA radiation-safety guide, and the DICOM Standard. Source pointers are not copied source content. Restricted or version-sensitive sources remain review-gated; exact release, license, and permitted-use terms must be confirmed before ingestion or redistribution.

## JSON sanity check

Performed a local JSON parse and deterministic count check after writing the artifact.

Expected results:

| Check | Result |
|---|---:|
| Valid JSON | PASS |
| Disciplines | 8 |
| Subdisciplines | 35 |
| Atomicity candidates | 35 |
| Open review items | 10 |
| Duplicate discipline IDs | 0 |
| Duplicate subdiscipline IDs | 0 |
| Duplicate atomicity candidate IDs | 0 |
| Semantic content in approved state | 0 |
| Large dataset / engine / training artifact | 0 |

Content-mode counts are 12 `educational_general` and 23 `clinical_guidance_sensitive`. Review-state counts are 79 `unreviewed` semantic records and 0 promoted records. The 79 consists of the domain, 8 disciplines, 35 subdisciplines, and 35 atomicity candidates.

## Next safe gate

Do not expand recursively or generate training data yet. First obtain domain review for the 10 open items, lock source/version/licensing decisions, and agree on an atomicity rubric. Only then should a separate, explicitly authorized task consider schema alignment or any future content pipeline.

## Evidence status

`PARTIAL / DISCOVERY-ONLY`: the JSON is structurally sanity-checked and source-pointers are recorded. Clinical semantic approval, authoritative hierarchy status, licensing clearance, Arabic terminology approval, and production/training readiness remain unresolved.
