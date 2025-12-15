Clinical NLP: Disease Category Classification from SOAP Notes
Overview

This project builds an end-to-end Natural Language Processing (NLP) pipeline to classify unstructured SOAP-style clinical notes into high-level disease categories commonly used in clinical triage and healthcare analytics.

The goal is not to predict a final diagnosis, but to automatically assign clinically meaningful disease buckets (e.g., respiratory, GI/GU, cardiac) from raw physician notes. This mirrors real-world healthcare workflows such as triage support, cohort identification, and downstream clinical analytics.

The project emphasizes clinical realism, explainability, and rigorous evaluation, including external validation and error analysis.

Problem Statement

Clinical notes in Electronic Health Records (EHRs) are largely unstructured and vary widely in writing style, length, and terminology. While structured diagnosis codes exist, they are often unavailable or delayed in real-time workflows.

This project addresses the following problem:

Can we reliably classify free-text SOAP notes into high-level disease categories using classical NLP and machine-learning techniques?

Dataset
Primary Dataset

~10,000 de-identified SOAP-style clinical notes

Each note contains narrative sections resembling:

Subjective (symptoms, complaints)

Objective (exam findings, labs)

Assessment

Plan

Notes are long, detailed, and written in clinician language

External Dataset (Option B)

27 short clinical case summaries

Used strictly for external validation

Structurally and stylistically different from SOAP notes

Disease Categories

To maintain clinical relevance and statistical stability, notes were ultimately mapped into six high-level disease classes:

Respiratory

GI / GU

Neurological

Cardiac

Musculoskeletal

General

These categories reflect common triage-level groupings rather than textbook specialties.

NLP Pipeline
1. Text Preprocessing

Lowercasing and whitespace normalization

Removal of punctuation and stopwords

Token standardization

Extensive medical abbreviation expansion using a custom dictionary derived from clinical documents

2. Feature Engineering

TF-IDF vectorization (word and bigram features)

Vocabulary learned exclusively from SOAP-style notes

3. Modeling

Two models were trained and compared:

Logistic Regression (baseline)

XGBoost multi-class classifier (final model)

XGBoost significantly outperformed the baseline and was selected for final evaluation.

In-Domain Model Performance

On held-out SOAP-note test data:

Overall accuracy ~90%

Strong weighted F1 score

High performance for respiratory, GI/GU, neurological, and cardiac classes

Lower performance for musculoskeletal and general classes due to class imbalance

This confirms the model effectively learns disease-specific language patterns within the SOAP note domain.

External Validation and Error Analysis
External Evaluation Results

When evaluated on the Option B dataset:

Accuracy dropped to ~19% using original labels

Improved to ~26% after clinician-informed relabeling

This degradation was expected and investigated in depth.

Root Cause: Domain Shift and Feature Sparsity

Quantitative analysis showed:

SOAP notes: hundreds to thousands of non-zero TF-IDF features per document

Option B notes: often fewer than 15 non-zero features

The Option B dataset consists of short, summary-style text that does not match the structure or vocabulary of SOAP notes. As a result:

Feature sparsity severely limits model signal

Predictions default to prior-heavy classes

Model behavior remains stable on in-domain data

This confirms the issue is domain mismatch, not model failure.

Label Noise and Clinician-Informed Relabeling

The external dataset also contained significant label noise, especially within the “general” category. To address this:

A clinician-informed relabeling framework was designed

Iterative refinement (v2 → v3) corrected semantic mismatches

Relabeling improved alignment but could not overcome domain shift alone

This mirrors real healthcare ML challenges, where label quality often limits achievable performance.

Key Takeaways

The model performs strongly on SOAP-style clinical notes

External generalization is limited by note structure and vocabulary mismatch

Label noise can be mitigated through clinical review

Domain adaptation requires representative training data, not blind retraining

This project intentionally documents these limitations rather than hiding them.

Future Work

Planned improvements include:

External validation using additional SOAP-style notes from different clinical settings

Exploration of more robust text representations (e.g., character-level n-grams)

Controlled retraining only after domain alignment is ensured

Ethical Considerations

All data used are de-identified

The model is intended for research and educational purposes

Outputs are not suitable for clinical decision-making without human oversight

License

This project is licensed under the Apache License 2.0, allowing academic and commercial use with proper attribution.

Author

Sai Kumar Chary Sripathi
Master’s in Health Data Science
Clinical NLP | Healthcare Analytics | Machine Learning
