# Pre-trained Models

## Distance Model (`distance_model.joblib`)

Predicts whether routing platforms will disagree on walking distance by more than 5%.

**Parameters:**
- Algorithm: Random Forest Classifier
- n_estimators: 300
- max_depth: 12
- max_features: sqrt
- min_samples_leaf: 3
- min_samples_split: 10
- class_weight: balanced
- random_state: 42

**Performance (20% holdout test set):**
- AUC: 87.8%
- Accuracy: 81.5%
- Sensitivity: 74.0%
- Specificity: 85.6%

---

## Time Model (`time_model.joblib`)

Predicts whether routing platforms will disagree on walking time by more than 20%.

**Parameters:**
- Algorithm: Random Forest Classifier
- n_estimators: 450
- max_depth: 12
- max_features: sqrt
- min_samples_leaf: 3
- min_samples_split: 10
- class_weight: balanced
- random_state: 42

**Performance (20% holdout test set):**
- AUC: 90.9%
- Accuracy: 82.1%
- Sensitivity: 83.2%
- Specificity: 80.9%

---

## Required Features (in order)

1. `Straight_Line_Distance_m`
2. `Origin_Road_Length_Density_m_km2`
3. `Dest_Intersection_Density_n_km2`
4. `Slope_Pct`
5. `Elevation_Difference_m`
6. `Population`

## Usage

```python
import joblib

model = joblib.load('distance_model.joblib')
predictions = model.predict(X)  # X must have the 6 features in order
probabilities = model.predict_proba(X)[:, 1]  # Probability of disagreement
```
