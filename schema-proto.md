# Clinical Data Repository – Core Data Model

## Entities

### Patient

- Patient ID
- MRN (Medical Record Number)
- Name
- Date of Birth
- Gender
- Ethnicity
- Contact Information
- Address
- Emergency Contact
- Insurance Information
- Consent Status
- Consent Date
- Consent Expiry

---

### Hospital / Facility

- Facility ID
- Facility Name
- Facility Code
- Facility Type
- Address
- Contact Information

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
- Diagnosis Date
- Diagnosis Status (Active, Resolved, Inactive)
- Doctor ID

---

### Comorbidity

- Comorbidity ID
- Patient ID
- Encounter ID
- Comorbidity Code
- Comorbidity Code System
- Comorbidity Description
- Recorded Date
- Doctor ID

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

### Medication / Treatment

- Medication ID
- Encounter ID
- Patient ID
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
- Treatment Type (Chemotherapy, Targeted Therapy, Hormone Therapy, Supportive Care)

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
- Attachment / DICOM Reference

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
- Variant Classification
- Testing Method
- Lab Performing Test
- Clinical Significance
- Associated Therapy

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
- Notes

---

### Doctor / Clinician

- Doctor ID
- Full Name
- Medical License Number
- Specialty
- Sub-specialty
- Department
- Facility ID
- Contact Information

---

### Specialty

- Specialty ID
- Specialty Name
- Specialty Code

---

## Relationships

```
Patient
├── has many Encounters
├── has many Diagnoses
├── has many Comorbidities
├── has many Medications / Treatments
├── has many Laboratory Results
├── has many Imaging Studies
├── has many Pathology Reports
├── has many Genomics Results
├── has many Progress Notes
├── has many Clinical Milestones / Outcomes
├── belongs to many Facilities (via Encounters)
└── has Consent Record

Encounter
├── belongs to Patient
├── belongs to Facility
├── belongs to Attending Doctor
├── has many Diagnoses
├── has many Procedures
├── has many Medications / Treatments
├── has many Laboratory Results
├── has many Imaging Studies
├── has many Pathology Reports
├── has many Progress Notes
└── has many Clinical Milestones / Outcomes

Diagnosis
├── belongs to Patient
├── belongs to Encounter
└── belongs to Doctor

Comorbidity
├── belongs to Patient
├── belongs to Encounter
└── belongs to Doctor

Procedure
├── belongs to Patient
├── belongs to Encounter
└── belongs to Performing Doctor

Medication / Treatment
├── belongs to Patient
├── belongs to Encounter
└── belongs to Prescribing Doctor

Laboratory Result
├── belongs to Patient
├── belongs to Encounter

Imaging Study
├── belongs to Patient
├── belongs to Encounter
└── belongs to Performing Radiologist

Pathology
├── belongs to Patient
├── belongs to Encounter
└── belongs to Pathologist

Genomics
├── belongs to Patient
├── belongs to Encounter

Progress Note
├── belongs to Patient
├── belongs to Encounter
└── belongs to Author Doctor

Clinical Milestone / Outcome
├── belongs to Patient
├── belongs to Encounter
├── may belong to Diagnosis
└── may belong to Treatment

Doctor
├── belongs to Specialty
├── belongs to Facility
└── has many Encounters (as Attending Doctor)

Facility
├── has many Encounters
└── has many Doctors
```

---

## Additional Considerations

| Entity | Notes |
|--------|-------|
| **Consent Record** | Tracks patient consent for research use, essential for regulatory compliance |
| **Audit Log** | Tracks who accessed what data and when (not an entity in the core model, but needed for governance) |
| **Data Source Tracking** | Indicates which source system each record came from, helps with data quality issues |
| **Diagnosis Code Mapping** | Maps between different coding systems (ICD-10, SNOMED-CT) for cross-system consistency |
| **Medication Code Mapping** | Maps between RxNorm, ATC, and local codes |
| **Date Precision** | Some clinical dates may be approximate (e.g., "circa 2020", month only, partial dates) – consider how to handle |

---

This schema covers the entities needed to support:

- **Cohort search** by diagnosis, procedure, medication, doctor, specialty
- **Longitudinal patient view** across encounters, treatments, investigations, outcomes
- **Multi-hospital capability** via the Facility entity
- **Research-grade design** with consent tracking and audit capabilities
- **Extensibility** for future disease domains beyond lung cancer