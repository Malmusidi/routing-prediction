# Predicting Pedestrian Routing Disagreements Among Web-Based Platforms

A machine learning framework to predict when Google Maps, ArcGIS, and OpenRouteService will produce divergent pedestrian travel time and distance estimates.

## Quick Start

```python
import joblib
import pandas as pd

# Load pre-trained models
distance_model = joblib.load('models/distance_model.joblib')
time_model = joblib.load('models/time_model.joblib')

# Prepare your features (must be in this exact order)
features = df[[
    'Straight_Line_Distance_m',
    'Origin_Road_Length_Density_m_km2',
    'Dest_Intersection_Density_n_km2',
    'Slope_Pct',
    'Elevation_Difference_m',
    'Population'
]]

# Predict disagreement (1 = platforms will likely disagree)
distance_disagreement = distance_model.predict(features)
time_disagreement = time_model.predict(features)
```

## Feature Definitions

| Feature | Definition | Unit | Source |
|---------|------------|------|--------|
| Straight_Line_Distance_m | Haversine distance between O-D | meters | Calculated |
| Origin_Road_Length_Density_m_km2 | Total road length in 400m buffer around origin | m/km² | OSM via OSMnx |
| Dest_Intersection_Density_n_km2 | Intersections (≥3 streets) in 400m buffer around destination | n/km² | OSM via OSMnx |
| Slope_Pct | (Elevation_dest - Elevation_origin) / Distance × 100 | % | SRTM DEM |
| Elevation_Difference_m | Elevation change between O-D | meters | SRTM DEM |
| Population | Census block group population at origin | count | US Census |

## Notebooks

- `01_generate_predictors.ipynb` - Generate the 6 features for your O-D pairs
- `02_train_models.ipynb` - Replicate model training on your own data
- `03_apply_pretrained_models.ipynb` - Apply pre-trained models to new data

## Requirements

```bash
pip install scikit-learn pandas numpy geopandas osmnx geopy earthengine-api census pygris joblib
```

## Citation

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

MIT License
