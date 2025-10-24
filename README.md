![Логотип](https://github.com/AndrewBioChem/NEUROML_FCD/blob/main/Adobe%20Express%20-%20file.png)

# Detection of brain area with Focal Cortical Dysplasia
This repository consists of main Jupyter notebook with data exploration and basic classifiers usage

## Problem
Focal cortical dysplasia (FCD) is one of the most common causes of drug-resistant epilepsy. However, its diagnosis remains extremely difficult, especially in mild cases. Traditional visual assessment methods, such as T1w, T2w, and FLAIR, strongly depend on the radiologist's expertise and often fail to detect subtle microstructural changes with the naked eye.

## Task
Detect FCD regions on MRI-derived radiomics data using ML classifiers. The objective was to develop a model with **zero false negatives per patient** (no missed patients) during cross-validation (CV), and to **minimize false positives (FP)**.

## Goal
Build a patient-wise machine-learning pipeline to reduce the area of manual data inspection by a radiologist.

## Initial dataset information
- Total samples: 11088
- Healthy samples: 10660
- FCD samples: 428
- Unique subjects: 168
- Unique brain areas: 66
- Total radiomics features: 280

## Data exploration
The dataset contained region-wise radiomics features derived from several MRI modalities (T1, T2, FLAIR). Each region was labeled as either FCD-positive or control. The MRI-derived radiomics dataset included 279 features extracted from T1-weighted, T2-weighted, and FLAIR images across 168 subjects. The dataset contained both healthy and FCD-affected 66 brain regions per subject. Each patient had several labeled regions.

*Statistics*
The composition of the dataset, feature distributions, and class balance were explored. Statistically significant radiomic differences between FCD and control tissue were identified through statistical testing (Mann-Whitney U with FDR correction). The effect size approximated group differences in size and direction. Top features were ranked by adjusted p-values and effect sizes.

**FLAIR** 
![image](https://github.com/AndrewBioChem/NEUROML_FCD/blob/main/2025-10-24_14-38-12.png)
This difference reflects the increased variability in the size of local clusters, which means heterogeneity of cortical organization (*original_gldm_DependenceVariance*), as well as a decrease in uniformity depending on the neighborhood, which is consistent with the general heterogeneity of tissue in FCD (*original_gldm_DependenceNonUniformityNormalized*)

**T1w and T2w modalities**
![image](https://github.com/AndrewBioChem/NEUROML_FCD/blob/main/2025-10-24_15-01-06.png)
A larger average for *original_gldm_LargeDependenceHighGrayLevelEmphasis_T1* indicates large homogeneous spots of high intensity on T1, which is a practical case for FCD. A drop *original_glrlm_ShortRunEmphasis_T2* indicates a characteristic tissue disorganization and loss of normal layering, which often occurs with FCD at the border of gray and white matter.

## Model pipeline 
1) Applied a **StandardScaler** to 279 features to make radiomics measures comparable (volume fearure droped)
2) **Held out 30% of subjects** as a final test set (no overlap with training set) to evaluate generalization\
   Train subjects: 117 | Test subjects: 51\
   Train samples: 7722 | Test samples: 3366
3) Model selection with a **GroupKFold (n_splits=5)** so that every fold contained **only whole patients** and no region from the same subject appeared in both training and validation sets. 
4) Dataset is highly imbalanced, **RandomUnderSampler** was used on the training portion of each fold to rebalance classes for model fitting.
5) GridSearch for list of classifiers:
   - *Logistic Regression*: C, penalty
   - *SVM*: C ,kernel,  gamma
   - *KNN*: n_neighbors, p, weights
   - *Gradient Boosting*: loss, learning_rate, n_estimators, subsample, criterion
   - *Random Forest*: n_estimators , criterion , max_depth, max_features, class_weight
   - *XGBoost*: n_estimators , max_depth, learning_rate
6) Optimized a decision threshold per fold using the rule **False Negative patients == 0** and selection the one that minimize fold **False Positives**.

## Results

   
