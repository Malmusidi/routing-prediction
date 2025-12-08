# Predicting Pedestrian Routing Disagreements Among Web-Based Platforms

A machine learning framework to predict when Google Maps, ArcGIS, and OpenRouteService will produce divergent pedestrian travel time and distance estimates.



The framework uses Random Forest models trained on 10,000 residential-to-bus-stop routes in Athens-Clarke County, Georgia.

## Model Performance

| Model | AUC | Accuracy | Threshold |
|-------|-----|----------|-----------|
| Distance Disagreement | 87.8% | 81.5% | 5% |
| Time Disagreement | 90.9% | 82.1% | 20% |

## Quick Start: Using Pre-Trained Models

If you just want to **apply the models** to your own O-D pairs:

```python
import joblib
import pandas as pd

# Load pre-trained models
distance_model = joblib.load('models/distance_model.joblib')
time_model = joblib.load('models/time_model.joblib')

# Your data needs these 6 features:
# 1. Straight_Line_Distance_m - Haversine distance between O-D (meters)
# 2. Origin_Road_Length_Density_m_km2 - Road length per km² in 400m buffer around origin
# 3. Dest_Intersection_Density_n_km2 - Intersections per km² in 400m buffer around destination
# 4. Slope_Pct - (Elevation_dest - Elevation_origin) / Distance * 100
# 5. Elevation_Difference_m - Elevation difference between O-D (meters)
# 6. Population - Block group population at origin

# Make predictions
features = df[['Straight_Line_Distance_m', 'Origin_Road_Length_Density_m_km2',
               'Dest_Intersection_Density_n_km2', 'Slope_Pct', 
               'Elevation_Difference_m', 'Population']]

distance_disagreement = distance_model.predict(features)  # 1 = disagreement likely
time_disagreement = time_model.predict(features)          # 1 = disagreement likely
```

See `notebooks/03_apply_pretrained_models.ipynb` for a complete walkthrough.

## Repository Structure

```
├── README.md
├── requirements.txt
├── LICENSE
│
├── models/
│   ├── distance_model.joblib    # Pre-trained distance disagreement model
│   └── time_model.joblib        # Pre-trained time disagreement model
│
├── notebooks/
│   ├── 01_generate_predictors.ipynb   # Generate the 6 predictor features
│   ├── 02_train_models.ipynb          # Train models (for replication)
│   └── 03_apply_pretrained_models.ipynb # Use pre-trained models (recommended)
│
└── data/
    └── sample_od_pairs.csv      # Example O-D pairs for testing
```

## Installation

### Option 1: Google Colab (Recommended)

Open any notebook directly in Colab - dependencies will be installed automatically.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

### Option 2: Local Installation

```bash
git clone https://github.com/Malmusidi/routing-prediction.git
cd routing-prediction
pip install -r requirements.txt
```

## Notebooks

### 1. Generate Predictors (`01_generate_predictors.ipynb`)

Calculates the 6 predictor features for your O-D pairs using:
- **OSMnx** for network metrics (road density, intersection density)
- **Google Earth Engine** for topographic data (elevation, slope)
- **US Census API** for demographic data (population)

**Requirements:**
- Origin/destination coordinates (latitude, longitude)
- Google Earth Engine account (free)
- US Census API key (free)

### 2. Train Models (`02_train_models.ipynb`)

Replicates the model training process from the paper. Use this if you want to:
- Train models on your own study area
- Modify the classification thresholds
- Experiment with different features or algorithms

### 3. Apply Pre-Trained Models (`03_apply_pretrained_models.ipynb`)

**Start here if you just want to use the framework.** This notebook:
- Loads the pre-trained models
- Shows how to generate features for new O-D pairs
- Makes predictions and interprets results

## Feature Definitions

| Feature | Definition | Unit | Source |
|---------|------------|------|--------|
| Straight_Line_Distance_m | Haversine distance between O-D | meters | Calculated |
| Origin_Road_Length_Density_m_km2 | Total road length in 400m buffer around origin | m/km² | OSM via OSMnx |
| Dest_Intersection_Density_n_km2 | Intersections (≥3 streets) in 400m buffer around destination | n/km² | OSM via OSMnx |
| Slope_Pct | (Elev_dest - Elev_origin) / Distance × 100 | % | SRTM DEM |
| Elevation_Difference_m | Absolute elevation change | meters | SRTM DEM |
| Population | Census block group population at origin | count | US Census |

## Interpreting Predictions

**Distance Model (5% threshold):**
- `0` = Platforms likely to agree (distance estimates within 5%)
- `1` = Platforms likely to disagree (distance estimates differ by >5%)

**Time Model (20% threshold):**
- `0` = Platforms likely to agree (time estimates within 20%)
- `1` = Platforms likely to disagree (time estimates differ by >20%)

The higher threshold for time reflects inherent differences in walking speed assumptions between platforms (Google Maps ~4.5 km/h vs. ORS/ArcGIS 5 km/h).

## Key Findings

**Distance disagreements** are primarily predicted by:
- Network complexity (road density + intersection density = 40% importance)
- Straight-line distance (26% importance)

**Time disagreements** are primarily predicted by:
- Topographic features (slope + elevation = 39% importance)
- Straight-line distance (20% importance)

## Limitations

- Models trained on Athens-Clarke County, Georgia (small-to-medium US city)
- Performance degrades for routes shorter than 500m
- Binary classification only (does not predict magnitude of disagreement)
- Does not indicate which platform is more accurate

## Citation

If you use this framework, please cite:

```bibtex
@article{almusidi2025routing,
  title={Predicting Pedestrian Routing Disagreements Among Web-Based Platforms: 
         A Machine Learning Framework for Accessibility Research},
  author={Almusidi, M.},
  journal={TBD},
  year={2025}
}
```

## License

MIT License - see LICENSE file for details.

## Contact

For questions or issues, please open a GitHub issue.
