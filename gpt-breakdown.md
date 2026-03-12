# Clinical Research Data Platform Prototype

## Overview

This document describes the architecture and design of a prototype system aimed at addressing operational and research pain points faced by the Clinical Research Coordinator (CRC) team.

The goal is to build a minimal platform that enables:

* Faster **cohort discovery**
* **Reproducible research queries**
* **Cross-patient and longitudinal analysis**
* Reduced reliance on **manual chart reviews**

The proposed architecture is based on an **ETL pipeline feeding a standardized clinical data model**, enabling structured queries and cohort discovery tools.

---

# 1. Technical Terms & Glossary

### Cohort

A **group of patients who share specific characteristics** and are studied together.

Example:

* Patients diagnosed with lung cancer
* Patients treated with a specific chemotherapy regimen
* Patients within a specific age range

---

### Cohort Discovery

The process of **identifying patients that match study criteria** from clinical datasets.

Example filters:

* Diagnosis
* Medication
* Lab values
* Age
* Time period

---

### ETL Pipeline

ETL stands for:

* **Extract** – pull data from source systems
* **Transform** – clean and standardize data
* **Load** – store data into a research database

ETL pipelines convert fragmented clinical data into a **unified research dataset**.

---

### Clinical Data Warehouse

A centralized database containing **integrated clinical data** from multiple hospital systems.

It allows researchers to run structured queries across patients.

---

### Standard Clinical Data Model

A standardized database schema used to represent clinical information consistently.

Example models include the OMOP Common Data Model developed by Observational Health Data Sciences and Informatics.

---

### Cohort Builder

A tool that allows researchers to **construct patient cohorts using filters rather than writing SQL queries**.

Example tool: ATLAS.

---

# 2. System Architecture

The prototype architecture consists of the following layers:

```
Clinical Source Systems
(EMR / LIS / PACS / Pharmacy)
        │
        ▼
Data Extraction Layer
        │
        ▼
ETL Pipeline
        │
        ▼
Standard Clinical Data Model (OMOP)
        │
        ▼
Research Database
        │
        ▼
Cohort Builder Interface
```

This architecture separates concerns between:

* Data ingestion
* Data standardization
* Data analysis

---

# 3. Cohort Builder

The cohort builder provides a visual interface that allows users to define study populations.

Researchers define criteria such as:

* Diagnosis
* Medication exposure
* Laboratory results
* Age
* Time ranges

Example interface logic:

```
Diagnosis = Lung Cancer
AND
Drug Exposure = Cisplatin
AND
Age > 50
AND
Treatment Date BETWEEN 2021–2024
```

Output:

```
Cohort Size: 247 patients
```

The cohort definition can then be saved and reused for future studies.

---

# 4. Cohort Building Examples

### Example 1 — Oncology Treatment Outcome

Objective:

Evaluate outcomes for patients treated with a specific chemotherapy regimen.

Criteria:

```
Diagnosis: Stage II Colon Cancer
Drug Exposure: FOLFOX
Treatment Date: 2022–2024
Age: > 40
```

Result:

```
Cohort Size: 183 patients
```

---

### Example 2 — Drug Safety Analysis

Objective:

Identify patients who developed adverse events after receiving a drug.

Criteria:

```
Drug Exposure: Cisplatin
Adverse Event: Kidney Injury
Lab Measurement: Creatinine > 1.5
Time Window: Within 30 days of treatment
```

---

### Example 3 — Longitudinal Outcome Study

Objective:

Analyze recurrence rates following surgery.

Criteria:

```
Diagnosis: Breast Cancer
Procedure: Tumor Resection
Follow-up Period: 5 years
Outcome: Recurrence diagnosis
```

---

# 5. ETL Pipeline Design

The ETL pipeline transforms data from source systems into the standardized research schema.

## ETL Stages

### Extract

Data is collected from hospital systems such as:

* EMR
* Laboratory Information System (LIS)
* Radiology System (PACS)
* Pharmacy System

Extraction methods may include:

* API calls
* scheduled database exports
* HL7 message feeds
* file transfers

---

### Transform

Transformation includes:

* data cleaning
* field normalization
* mapping local codes to standard concepts
* resolving patient identifiers
* removing duplicates

Example transformation:

```
Source diagnosis codes
→ standardized medical concepts
```

---

### Load

After transformation, the data is inserted into the OMOP data model tables.

Key tables include:

```
person
condition_occurrence
drug_exposure
measurement
procedure_occurrence
visit_occurrence
```

---

# 6. Adapter Design for Source Systems

Because hospital systems vary widely, the ETL pipeline should use **modular adapters**.

Each adapter handles a specific source system.

Example structure:

```
adapters/
    emr_adapter.py
    lis_adapter.py
    pacs_adapter.py
    pharmacy_adapter.py
```

Responsibilities of adapters:

* connect to source system
* extract relevant datasets
* convert data into intermediate formats

Adapters allow the pipeline to be **extensible and maintainable** when integrating additional systems.

---

# 7. Research Data Layer

After transformation, data is stored in a research database implemented using PostgreSQL.

This database provides:

* structured querying
* cohort analysis
* longitudinal tracking
* reproducible datasets

Researchers and analytics tools access this layer for analysis.

---

# 8. Best Practices for Clinical Research Data Systems

Successful research platforms typically separate system responsibilities into layers:

```
Data Ingestion
      ↓
Raw Data Storage
      ↓
Data Standardization
      ↓
Research Data Warehouse
      ↓
Analytics and Cohort Discovery
```

This layered architecture provides:

* reproducibility
* scalability
* interoperability
* improved research efficiency

---

# 9. Challenges

### Data Standardization

Clinical data is often inconsistent across systems.

Example issues:

* multiple diagnosis coding systems
* inconsistent drug names
* differing lab formats

Standard terminology mapping is required.

---

### Data Quality

Incomplete or inconsistent data may lead to unreliable research results.

Quality checks must be implemented within the ETL pipeline.

---

### System Integration

Hospital systems may change over time.

ETL adapters must be designed to handle:

* schema changes
* API updates
* data format changes

---

### Clinical Context

Technical teams must collaborate with clinicians to ensure:

* correct interpretation of clinical fields
* accurate mapping of medical events
* valid research datasets

---

# 10. Future Enhancements

Potential extensions for the platform include:

* automated cohort alerts
* machine learning analysis
* clinical trial matching
* cross-institution research collaboration
* integration with clinical registries

---

# Conclusion

The proposed architecture replaces manual chart review workflows with a **systematic, queryable research data platform**.

By combining an ETL pipeline with a standardized clinical data model and cohort discovery tools, the system enables:

* faster research execution
* reproducible studies
* scalable clinical data analysis
* improved research productivity for CRC teams
