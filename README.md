# Detection of brain area with Focal Cortical Dysplasia
This repository consists of main jupyter notebook with data exploration and basic classifiers usage

## Problem
Focal cortical dysplasia (FCD) is one of the most common causes of drug-resistant epilepsy. However, its diagnosis remains extremely difficult, especially in mild cases. Traditional visual assessment methods such as T1w, T2w, and FLAIR strongly depend on the radiologist's expertise and often fail to detect subtle microstructural changes with the naked eye.
## Task
Detect FCD regions on MRI-derived radiomics data using ML classifiers. The objective was to develop a model with **zero false negatives per patient** (no missed patients) during cross-validation (CV), and to **minimize false positives (FP)**.
## Goal
Build a patient-wise machine-learning pipeline to reduces the area of manual data inspection by a radiologist.

## Data exploration
The dataset contained region-wise radiomics features derived from several MRI modalities (T1, T2, FLAIR). Each region was labeled as either FCD-positive or control. MRI-derived radiomics dataset included 280 features extracted from T1-weighted, T2-weighted and FLAIR images across 168 subjects. The dataset contained both healthy and FCD-affected brain regions segmented into 66 cortical areas per subject. Each patient had several labeled regions.

*Statistics*
The composition of the dataset, feature distributions, and class balance were explored. Statistically significant radiomic differences between FCD and control tissue were identified through statistical testing (Mann-Whitney U with FDR correction). The effect size approximated group differences in size and direction. Top features were ranked by adjusted p-values and effect sizes.
