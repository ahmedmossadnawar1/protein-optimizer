# PlantProtein AI: Protein Blend Optimizer

A Flask web app that generates plant-based protein blends and scores them against the amino acid profile of egg protein. It combines constrained numerical optimization (scipy SLSQP) with a machine learning model trained on food composition data.

## What It Does

- Loads a dataset of 603 foods with amino acid profiles (`merged.xlsx`)
- Trains a regression model at startup to predict blend scores
- Builds blends under constraints (for example: no ingredient above 60% of a blend, ingredients under 10g protein/100g capped at 5%)
- Scores blends with a weighted combination of cosine similarity to the egg amino acid profile, a PDCAAS-style calculation, and the ML model's prediction
- Generates recipe suggestions from a blend

## Tech Stack

- Python, Flask
- scipy, scikit-learn, pandas, numpy
- Single-page frontend (`index.html`)

## Project Structure

```
├── app.py              # Flask backend: optimization, ML training, API
├── index.html          # Frontend UI
├── ml_model.py         # ML training pipeline
├── merged.xlsx         # Food dataset (603 foods)
├── requirements.txt    # Dependencies
└── README.md           # This file
```

## Installation

```bash
pip install -r requirements.txt
```

## Run

```bash
python app.py
```

The model trains at startup (about a minute), then the app is available at http://localhost:5000

## API Overview

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Web UI |
| `/api/status` | GET | Server and model status |
| `/api/metrics` | GET | Model metrics and feature importance |
| `/api/blends` | GET | Generate optimized blends |
| `/api/predict_custom` | POST | Optimize a blend from selected ingredients |
| `/api/predict_food` | POST | Score a single food |
| `/api/generate_recipe` | POST | Generate a recipe from a blend |
| `/api/all_foods` | GET | List foods in the dataset |
| `/api/top_foods` | GET | Top foods by protein content |
| `/api/train` | POST | Retrain the model |
| `/api/add_data` | POST | Add extra food data files |
| `/api/extra_files` | GET | List added data files |
| `/api/remove_extra` | POST | Remove an added data file |

## Model Performance

On the most recent local training run the regression model reached R² = 0.975 (cross-validated R² = 0.963) against its own synthetic blend scoring data. This measures how well the model reproduces its own scoring function; it is not a measure of nutritional accuracy.

## Limitations

- Blend scores are computational estimates based on the dataset and scoring function in this repo. They are not validated dietary, medical, or clinical guidance.
- The amino acid reference data comes from the bundled dataset and has not been independently verified.
- This is a graduation project, not a production nutrition tool.

## Author

Ahmed Mossad Nawar
