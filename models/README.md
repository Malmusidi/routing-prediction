# Model Files

This directory should contain the pre-trained Random Forest models:

- `distance_model.joblib` - Predicts distance disagreement (>5% threshold)
- `time_model.joblib` - Predicts time disagreement (>20% threshold)

## How to Add Models

1. Train the models using `notebooks/02_train_models.ipynb`
2. The notebook will save the models as `.joblib` files
3. Copy those files to this directory

## Model Details

**Distance Model:**
- Algorithm: Random Forest (300 trees)
- Threshold: 5% disagreement
- Expected Accuracy: ~81.5%
- Expected AUC: ~87.8%

**Time Model:**
- Algorithm: Random Forest (450 trees)
- Threshold: 20% disagreement
- Expected Accuracy: ~82.1%
- Expected AUC: ~90.9%

## Required Features (in order)

1. `Straight_Line_Distance_m`
2. `Origin_Road_Length_Density_m_km2`
3. `Dest_Intersection_Density_n_km2`
4. `Slope_Pct`
5. `Elevation_Difference_m`
6. `Population`
