# Clinical Data Repository – Core Data Model

> **Note:** All entities carry the following common provenance fields (not repeated per entity):
>
> - Source System (originating system: EMR, LIS, PACS, Pharmacy, Chemotherapy Module, etc.)
> - Source Record ID (original record identifier in the source system)
> - Created Date
> - Updated Date

---

## Entities

### Patient

- Patient ID
- MRN (Medical Record Number)
- Name
- Date of Birth
- Gender
- Ethnicity
- Nationality
- Preferred Language
- Contact Information
- Address
- Emergency Contact
- Insurance Information
- Deceased Flag (Yes/No)
- Death Date

---

### Consent Record

- Consent ID
- Patient ID
- Consent Type (Research, Data Sharing, Biobank, Clinical Trial, etc.)
- Consent Status (Granted, Revoked, Expired, Pending)
- Granted Date
- Expiry Date
- Revocation Date
- Study / Purpose
- Consenting Doctor ID

---

### Hospital / Facility

- Facility ID
- Facility Name
- Facility Code
- Facility Type
- Address
- Contact Information

---

### Doctor / Clinician

- Doctor ID
- Full Name
- Medical License Number
- Specialty ID (FK to Specialty)
- Sub-specialty ID (FK to Specialty)
- Department
- Contact Information

---

### Doctor–Facility Assignment

- Assignment ID
- Doctor ID
- Facility ID
- Role (Primary, Visiting, Consultant)
- Start Date
- End Date

---

### Specialty

- Specialty ID
- Specialty Name
- Specialty Code

---

### Encounter / Visit

- Encounter ID
- Patient ID
- Facility ID
- Admission Date
- Discharge Date
- Encounter Type (Inpatient, Outpatient, Emergency, Day Care)
- Ward / Department
- Admission Type
- Discharge Status
- Discharge Disposition (Home, Rehab, Hospice, Deceased, Other)
- Attending Doctor ID
- Referring Doctor ID

---

### Diagnosis

- Diagnosis ID
- Encounter ID
- Patient ID
- Diagnosis Code
- Diagnosis Code System (ICD-10, SNOMED-CT, etc.)
- Diagnosis Description
- Diagnosis Type (Primary, Secondary, Admitting, Discharge)
- Diagnosis Category (New Diagnosis, Comorbidity, Historical)
- Diagnosis Date
- Diagnosis Status (Active, Resolved, Inactive)
- Doctor ID

> **Note:** Comorbidities are now captured as Diagnosis records with `Diagnosis Category = Comorbidity`. The former standalone Comorbidity entity has been merged here to eliminate redundancy while preserving the ability to filter and report on comorbidities separately.

---

### Cancer Staging

- Staging ID
- Patient ID
- Encounter ID
- Diagnosis ID
- Staging System (AJCC 8th Edition, etc.)
- Staging Type (Clinical cTNM, Pathological pTNM, Post-therapy yTNM)
- T Component
- N Component
- M Component
- Overall Stage Group (I, IA, IB, II, IIA, IIB, III, IIIA, IIIB, IIIC, IV, IVA, IVB)
- Staging Date
- Staging Doctor ID

---

### Procedure

- Procedure ID
- Encounter ID
- Patient ID
- Procedure Code
- Procedure Code System (ICD-9-CM, CPT, SNOMED-CT, etc.)
- Procedure Description
- Procedure Date
- Procedure Type
- Surgeon / Performing Doctor ID
- Anesthesia Type
- Outcome
- Duration

---

### Treatment Line / Regimen

- Regimen ID
- Patient ID
- Diagnosis ID
- Line of Therapy (1st Line, 2nd Line, 3rd Line, etc.)
- Regimen Name
- Intent (Curative, Palliative, Adjuvant, Neoadjuvant)
- Start Date
- End Date
- Status (Active, Completed, Discontinued, Changed)
- Discontinuation Reason
- Prescribing Doctor ID

---

### Medication / Treatment

- Medication ID
- Encounter ID
- Patient ID
- Regimen ID (FK to Treatment Line / Regimen, optional)
- Medication Name
- Medication Code (RxNorm, ATC, etc.)
- Dose
- Dose Unit
- Frequency
- Route (Oral, IV, IM, etc.)
- Start Date
- End Date
- Prescribing Doctor ID
- Indication
- Treatment Type (Chemotherapy, Targeted Therapy, Immunotherapy, Hormone Therapy, Supportive Care)

---

### Radiation Therapy

- Radiation Therapy ID
- Encounter ID
- Patient ID
- Regimen ID (FK to Treatment Line / Regimen, optional)
- Treatment Type (Definitive, Adjuvant, Palliative, Prophylactic)
- Radiation Site / Target
- Total Dose (Gy)
- Number of Fractions
- Start Date
- End Date
- Prescribing Doctor ID
- Performing Technologist ID

---

### Laboratory Result

- Lab Result ID
- Encounter ID
- Patient ID
- Test ID
- Test Name
- Test Code (LOINC)
- Component Name
- Component Code
- Result Value
- Result Unit
- Reference Range
- Result Date
- Specimen Type
- Specimen Collection Date
- Performing Lab
- Abnormal Flag

---

### Imaging Study

- Imaging Study ID
- Encounter ID
- Patient ID
- Modality (CT, MRI, X-Ray, Ultrasound, PET-CT, etc.)
- Study Description
- Study Code
- Study Date
- Referring Doctor ID
- Performing Radiologist ID
- Findings
- Impression
- RECIST Response (CR, PR, SD, PD, NE)
- Attachment / DICOM Reference

---

### Response Assessment

- Assessment ID
- Patient ID
- Encounter ID
- Regimen ID (FK to Treatment Line / Regimen)
- Assessment Date
- Assessment Type (Radiological, Pathological, Clinical)
- Assessment Criteria (RECIST 1.1, iRECIST, Pathological Response, etc.)
- Response Category (Complete Response, Partial Response, Stable Disease, Progressive Disease, Not Evaluable)
- Assessing Doctor ID
- Notes

---

### Pathology / Biopsy

- Pathology ID
- Encounter ID
- Patient ID
- Specimen Type
- Specimen Collection Date
- Collection Method
- Pathologist ID
- Report Date
- Histology / Diagnosis
- Grade
- Stage (TNM Staging where applicable)
- Molecular Markers
- Ki-67 Index
- PD-L1 Expression (TPS %)
- Biomarker Results

---

### Genomics / Molecular Testing

- Genomics ID
- Encounter ID
- Patient ID
- Test Name
- Test Date
- Gene Name
- Variant Detected
- Variant Classification (Pathogenic, Likely Pathogenic, VUS, Likely Benign, Benign)
- Testing Method
- Lab Performing Test
- Clinical Significance
- Associated Therapy

> **Lung cancer actionable targets:** EGFR, ALK, ROS1, BRAF, KRAS, NTRK, MET, RET, HER2.
> These are captured via the Gene Name + Variant Detected fields. Each actionable mutation is stored as a separate Genomics record per gene tested.

---

### Progress Note

- Note ID
- Encounter ID
- Patient ID
- Note Type (Progress Note, Consultation Note, Discharge Summary, Admission Note)
- Note Date
- Author Doctor ID
- Note Title
- Note Content
- Subjective
- Objective
- Assessment
- Plan

---

### Clinical Milestone / Outcome

- Milestone ID
- Patient ID
- Encounter ID
- Milestone Type
- Milestone Description
- Milestone Date
- Associated Diagnosis ID
- Associated Treatment ID
- Outcome Status
- Survival Status (Alive, Deceased, Lost to Follow-up, Unknown)
- Last Known Alive Date
- Notes

---

### Social History

- Social History ID
- Patient ID
- Smoking Status (Current, Former, Never, Unknown)
- Smoking Pack-Years
- Smoking Start Date
- Smoking Quit Date
- Alcohol Use (None, Light, Moderate, Heavy, Unknown)
- Occupational Exposure
- Air Pollution Exposure
- Recorded Date
- Recorded Doctor ID

---

### Family History

- Family History ID
- Patient ID
- Relationship (Parent, Sibling, Child, Grandparent, Aunt/Uncle, Cousin, Other)
- Side (Maternal, Paternal, Both, Unknown)
- Cancer Type
- Age at Diagnosis
- Recorded Date

---

### Performance Status

- Performance Status ID
- Patient ID
- Encounter ID
- Assessment Date
- Score Type (ECOG, Karnofsky)
- Score Value
- Assessing Doctor ID

---

### Adverse Event / Toxicity

- Adverse Event ID
- Encounter ID
- Patient ID
- Treatment ID
- Event Term
- CTCAE Term ID
- Severity Grade (1-5)
- Start Date
- End Date
- Outcome (Resolved, Ongoing, Fatal, Unknown)
- Serious Adverse Event Flag (Yes/No)
- Attribution (Unrelated, Unlikely, Possible, Probable, Definite)
- Doctor ID
- Reported Date

---

### Allergy

- Allergy ID
- Patient ID
- Allergen
- Allergen Type (Drug, Food, Environmental, Other)
- Reaction Type
- Severity (Mild, Moderate, Severe)
- Recorded Date
- Recorded Doctor ID

---

## Relationships

```
Patient
├── has many Encounters
├── has many Consent Records
├── has many Diagnoses (including Comorbidities)
├── has many Cancer Stagings
├── has many Procedures
├── has many Treatment Lines / Regimens
├── has many Medications / Treatments
├── has many Radiation Therapy records
├── has many Laboratory Results
├── has many Imaging Studies
├── has many Response Assessments
├── has many Pathology Reports
├── has many Genomics Results
├── has many Progress Notes
├── has many Clinical Milestones / Outcomes
├── has many Social History records
├── has many Family History records
├── has many Performance Status assessments
├── has many Adverse Events
├── has many Allergies
└── belongs to many Facilities (via Encounters)

Consent Record
├── belongs to Patient
└── belongs to Consenting Doctor

Encounter
├── belongs to Patient
├── belongs to Facility
├── belongs to Attending Doctor
├── has many Diagnoses
├── has many Cancer Stagings
├── has many Procedures
├── has many Medications / Treatments
├── has many Radiation Therapy records
├── has many Laboratory Results
├── has many Imaging Studies
├── has many Response Assessments
├── has many Pathology Reports
├── has many Genomics Results
├── has many Progress Notes
├── has many Clinical Milestones / Outcomes
├── has many Performance Status assessments
└── has many Adverse Events

Diagnosis
├── belongs to Patient
├── belongs to Encounter
├── belongs to Doctor
└── has many Cancer Stagings

Cancer Staging
├── belongs to Patient
├── belongs to Encounter
├── belongs to Diagnosis
└── belongs to Staging Doctor

Procedure
├── belongs to Patient
├── belongs to Encounter
└── belongs to Performing Doctor

Treatment Line / Regimen
├── belongs to Patient
├── belongs to Diagnosis
├── belongs to Prescribing Doctor
├── has many Medications / Treatments
├── has many Radiation Therapy records
└── has many Response Assessments

Medication / Treatment
├── belongs to Patient
├── belongs to Encounter
├── may belong to Regimen
└── belongs to Prescribing Doctor

Radiation Therapy
├── belongs to Patient
├── belongs to Encounter
├── may belong to Regimen
└── belongs to Prescribing Doctor

Laboratory Result
├── belongs to Patient
└── belongs to Encounter

Imaging Study
├── belongs to Patient
├── belongs to Encounter
└── belongs to Performing Radiologist

Response Assessment
├── belongs to Patient
├── belongs to Encounter
├── belongs to Regimen
└── belongs to Assessing Doctor

Pathology
├── belongs to Patient
├── belongs to Encounter
└── belongs to Pathologist

Genomics
├── belongs to Patient
└── belongs to Encounter

Progress Note
├── belongs to Patient
├── belongs to Encounter
└── belongs to Author Doctor

Clinical Milestone / Outcome
├── belongs to Patient
├── belongs to Encounter
├── may belong to Diagnosis
└── may belong to Treatment

Social History
├── belongs to Patient
└── belongs to Recorded Doctor

Family History
├── belongs to Patient

Performance Status
├── belongs to Patient
├── belongs to Encounter
└── belongs to Assessing Doctor

Adverse Event / Toxicity
├── belongs to Patient
├── belongs to Encounter
├── belongs to Treatment
└── belongs to Doctor

Allergy
├── belongs to Patient
└── belongs to Recorded Doctor

Doctor
├── has many Facility Assignments (via Doctor–Facility Assignment)
├── belongs to Specialty
├── may belong to Sub-specialty
└── has many Encounters (as Attending Doctor)

Doctor–Facility Assignment
├── belongs to Doctor
└── belongs to Facility

Facility
├── has many Encounters
└── has many Doctor Assignments (via Doctor–Facility Assignment)
```

---

## Additional Considerations

| Area | Notes |
|------|-------|
| **Audit Log** | Tracks who accessed what data and when (not an entity in the core model, but needed for governance) |
| **Data Source Tracking** | Now addressed via common provenance fields (Source System, Source Record ID) on all entities |
| **Diagnosis Code Mapping** | Maps between different coding systems (ICD-10, SNOMED-CT) for cross-system consistency |
| **Medication Code Mapping** | Maps between RxNorm, ATC, and local codes |
| **Date Precision** | Some clinical dates may be approximate (e.g., "circa 2020", month only, partial dates) – consider how to handle |
| **Comorbidity Migration** | Former Comorbidity entity merged into Diagnosis with `Diagnosis Category = Comorbidity`; existing comorbidity data should be migrated accordingly |

---

## Changes from Previous Version

| # | Change | Priority | Rationale |
|---|--------|----------|-----------|
| 1 | Added common provenance fields (Source System, Source Record ID, Created Date, Updated Date) to all entities | High | Data originates from EMR, LIS, PACS, pharmacy – traceability is essential per goal.md §6 |
| 2 | Fixed Doctor → Specialty relationship: replaced text fields with Specialty ID / Sub-specialty ID FKs | High | Enables cohort search by specialty (goal.md §4.1) |
| 3 | Added Deceased Flag, Death Date, Nationality, Preferred Language to Patient | High | Death Date critical for survival analysis; demographics needed for research |
| 4 | Promoted Consent to standalone entity; removed consent fields from Patient | High | Consent can be granted/revoked multiple times for different studies; per goal.md §6 regulatory requirements |
| 5 | Updated all relationship trees to include new entities | High | Structural completeness |
| 6 | Added Cancer Staging entity with T/N/M components | Medium | TNM staging at multiple time points is fundamental for oncology research; was previously a single text field in Pathology |
| 7 | Added Treatment Line / Regimen entity | Medium | "What was the 2nd-line regimen?" is a core oncology research question; no prior support for line-of-therapy |
| 8 | Added Response Assessment entity | Medium | Structured treatment response tracking (RECIST, pathological response) needed for outcomes analysis (goal.md §4.2) |
| 9 | Added PD-L1 Expression (TPS %) to Pathology | Medium | Key biomarker for lung cancer immunotherapy eligibility |
| 10 | Clarified Genomics entity covers lung cancer actionable targets (EGFR, ALK, ROS1, BRAF, KRAS, NTRK, MET, RET, HER2) | Medium | Pilot focus on lung cancer (goal.md §5) |
| 11 | Merged Comorbidity into Diagnosis with `Diagnosis Category` field | Medium | Eliminates redundancy; comorbidities are diagnoses with a different classification |
| 12 | Replaced single Doctor → Facility FK with Doctor–Facility Assignment junction entity | Low | Doctors practice across multiple hospitals (goal.md multi-hospital capability §5) |
| 13 | Added Discharge Disposition to Encounter | Low | Distinguishes clinical discharge status from disposition (home, hospice, deceased) |
| 14 | Added RECIST Response to Imaging Study | Low | Structured tumor response at the imaging level |
| 15 | Added Survival Status, Last Known Alive Date to Clinical Milestone / Outcome | Low | Required for survival analysis endpoints |
| 16 | Added Immunotherapy to Medication Treatment Type enum | Low | Common lung cancer treatment type was missing |

---

This schema covers the entities needed to support:

- **Cohort search** by diagnosis, procedure, medication, doctor, specialty
- **Longitudinal patient view** across encounters, treatments, investigations, outcomes
- **Multi-hospital capability** via the Facility entity and Doctor–Facility Assignment
- **Research-grade design** with consent tracking, data provenance, and audit capabilities
- **Lung cancer pilot readiness** with cancer staging, treatment lines, response assessment, PD-L1, and actionable genomic targets
- **Extensibility** for future disease domains beyond lung cancer
