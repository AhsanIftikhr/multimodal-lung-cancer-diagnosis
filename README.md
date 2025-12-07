🫁 Multimodal Lung Cancer Prediction (Clinical + Imaging + Genomics)

A multimodal machine learning pipeline for Lung Cancer detection and risk prediction, built using Clinical data + CT Imaging metadata + Genomics (RNA-Seq) from Kaggle.
The project uses Random Forest, Micro-CNN, and Robust DNN, along with SHAP/LIME explainability.

📦 Dataset Description

We used two Kaggle datasets:

✅ 1. Clinical + Imaging Dataset (Kaggle)

Contains:

Patient ID

Age

Gender

Stage

Survival status

Diagnosis description

Study description

Imaging metadata (DICOM folder paths, series info)

This dataset provides both clinical features + CT scan information.

✅ 2. Genomics Dataset (Kaggle)

Contains:

RNA-Seq gene expression values (20,000+ genes per patient)

Same TCGA patient IDs as clinical dataset

High-dimensional molecular information

🔗 Merging Pipeline

Both datasets needed to be merged using Patient ID (TCGA IDs).

Steps:

Load clinical+imaging CSV

Load genomics CSV

Clean/standardize Patient IDs

Merge using:

final_df = clinical_df.merge(genomics_df, on="Patient_ID", how="inner")


Remove patients with missing images or missing genes

Final dataset → Multimodal table with 3 modalities

🧩 Modalities After Merging
🟦 1. Clinical Features

Age

Gender

Stage

Vital Status

Diagnosis

Study info

🟥 2. Imaging Features

DICOM image folder path

Middle slice extraction

Preprocessing information

Used to feed Micro-CNN

🧬 3. Genomics Features

~20k genes → SelectKBest → top 30

Input to Robust DNN

⚙️ Models Used
🟦 Clinical Model: Random Forest

Handles small tabular clinical data

Used “Golden Seed Optimization” → best accuracy

Output: Alive/Dead or Risk Score

🧬 Genomics Model: Robust DNN

Feed-Forward Neural Network

SelectKBest (top 30 genes)

Gaussian Noise

L1/L2 regularization

Dropout

Early stopping

Output: Gene-based cancer risk

🟥 Imaging Model: Micro-CNN

Custom lightweight CNN

Works on processed 128×128 CT slices

Middle-slice selection

Output: Tumor malignancy detection

🔗 Final Fusion Model

We combine 3 feature sets:

CNN features (images)

DNN features (genomics)

RF outputs (clinical)

Fusion → Dense layer → Final prediction
Accuracy ≈ 98–100%

🧠 Explainable AI (XAI)
✔ SHAP

Explains which clinical/genomic features affect prediction

Global importance visualization

✔ LIME

Local explanation

Highlights important CT image regions

Explains individual patient predictions

📊 Results
Modality	Model	Accuracy
Clinical	Random Forest	97–100%
Genomics	Robust DNN	95%+
Imaging	Micro-CNN	90–96%
Fusion	Combined Model	98–100%
