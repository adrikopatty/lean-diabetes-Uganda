# Developing and Comparing Machine Learning Models for Detecting Lean Type 2 Diabetes in Uganda: Locally Trained versus Foreign-Trained Models

MSC 7103 Machine Learning, Practical Exam 2026, Makerere University
Adriko Patricia Lawuri and Kasadha Jesse

## Why lean type 2 diabetes?
About a third of Ugandans newly diagnosed with type 2 diabetes are **lean** (BMI < 25 kg/m²), and their diabetes is linked to low insulin secretion rather than obesity. Most diabetes prediction models, however, are trained on foreign data (such as US surveys or the Pima Indians dataset) in which diabetes is driven by obesity. Such models may learn that "a normal BMI means low risk" and miss lean Ugandan patients.

## Objective
To develop and compare machine learning models for detecting type 2 diabetes in Ugandan adults, with a focus on **lean adults**:
1. Which of five algorithms (Logistic Regression, Decision Tree, Random Forest, XGBoost, SVM) performs best on Ugandan data?
2. Do models trained on Ugandan data detect diabetes, especially in lean adults, better than models trained on foreign data?
3. Which features drive the predictions of foreign-trained and locally trained models?

## Key findings
- Lean adults make up **12%** of diabetics in the US data, but **28%** of the Ugandan outpatients with diabetes and **61%** of diabetics in the Ugandan national survey.
- A model trained on US data and used unchanged detected only **2 of 23** lean diabetic outpatients and **4 of 27** in the national survey.
- Logistic Regression trained on Ugandan data had the highest AUC of the five algorithms (0.67) and, with local features such as waist circumference and family history, detected **17 of 23** lean diabetics.
- SHAP shows why: the US model treats a normal BMI as low risk. In Ugandan patients, **age** was the clearest risk factor, while BMI was not a significant independent predictor.

## Data
| Role | Dataset | Diabetes defined by |
|---|---|---|
| Foreign (training) | CDC Diabetes Health Indicators, BRFSS 2015, United States ([UCI, id 891](https://archive.ics.uci.edu/dataset/891/cdc+diabetes+health+indicators)). Downloaded automatically by the notebook. | Self-report |
| Uganda, outpatients (training) | Kasujja et al. (2022), eastern Uganda outpatients ([Figshare, CC BY 4.0](https://doi.org/10.6084/m9.figshare.20152373)). **Included in this repository** in `data/uganda_figshare/`. | Oral glucose tolerance test or fasting glucose |
| Uganda, national (independent test) | WHO STEPS Survey, Uganda 2014 ([WHO NCD Microdata Repository](https://extranet.who.int/ncdsmicrodata/index.php/catalog/633)). Licensed data, **not included in this repository**: request access from WHO, then save as `data/uganda_steps/uga2014.dta`. | Fasting glucose or treatment |

## Approach
- **A. Foreign:** XGBoost trained on US data, applied to Ugandans with its US threshold and with a threshold chosen on Ugandan data.
- **B. Local:** five algorithms trained on Ugandan outpatients (stratified 5-fold cross-validation, repeated 10 times).
- **C. Pre-train and fine-tune:** the US XGBoost model trained further on Ugandan data.
- **D. Independent test:** all models tested on the WHO STEPS national survey.

Every result is reported for all patients and separately for **lean** and non-lean patients.

## Repository contents
| File | Description |
|---|---|
| `lean_diabetes_uganda.ipynb` | The full analysis: data loading, cleaning, exploration, experiments A-D, SHAP interpretation, and a final cell to screen a new patient |
| `data/uganda_figshare/hba1c_vs_fpg_validity_data.dta` | Kasujja et al. (2022) eastern Uganda outpatient data (CC BY 4.0) |
| `adriko_patricia.pdf` | The project report |
| `lean_diabetes_uganda.zip` | LaTeX source of the report (ACL template, bibliography and figures); upload to Overleaf and compile `acl_latex.tex` with pdfLaTeX |
| `requirements.txt` | Python packages needed to run the notebook |

The US data is downloaded automatically by the notebook. The WHO STEPS data must be requested from WHO and saved as `data/uganda_steps/uga2014.dta` before running the notebook.

## Running
```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook lean_diabetes_uganda.ipynb
```

## Data sources and acknowledgements
- Kasujja, F. X., Mayega, R. W., Daivadanam, M., Kiracho, E. E., Kusolo, R., & Nuwaha, F. (2022). Glycated haemoglobin and fasting plasma glucose tests in the screening of outpatients for diabetes and abnormal glucose regulation in Uganda: A diagnostic accuracy study. *PLoS ONE, 17*(8), e0272515. https://doi.org/10.1371/journal.pone.0272515. Data: https://doi.org/10.6084/m9.figshare.20152373
- World Health Organization and Ministry of Health Uganda. WHO STEPS Survey, Uganda 2014. WHO NCD Microdata Repository: https://extranet.who.int/ncdsmicrodata/index.php/catalog/633
- Centers for Disease Control and Prevention. (2017). CDC Diabetes Health Indicators [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C53919
