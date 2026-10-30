# IDS Packet Classifier

Machine-learning intrusion detection on network-flow data: explore the traffic, pick the most
useful features, and train classifiers that label each flow as benign or as one of several attack types.

The notebooks are a pipeline and are meant to be read in this order:

| Notebook | What it does |
|---|---|
| `Dataset Creation.ipynb` | Concatenates the daily flow CSVs (named `02-14-2018.csv` to `03-01-2018.csv` and so on) into one frame and writes a reduced dataset. |
| `Feature Extractor.ipynb` | Runs XGBoost-based feature selection over the data in chunks and saves the selected-feature files. |
| `Analysis.ipynb` | Exploratory analysis: attack-type counts, forward vs backward packet sizes, window-byte anomalies, and per-attack profiles (DDoS LOIC-HTTP, DoS Hulk, FTP brute force, SSH brute force). |
| `Model.ipynb` | Trains and evaluates models on the 23 selected features: SMOTE for class imbalance, `StandardScaler`, a 1D-CNN (Keras), a dense network, and a Random Forest. |

## Data

The dataset is **not included** in this repository. The notebooks read flow CSVs and generated
files (`concat_df.csv`, `reduced_dataset.csv`, `final_df2.csv`) that you need to supply or regenerate.
The daily file names match the CSE-CIC-IDS2018 captures, but the notebooks themselves do not name
the source, so check that before reusing the results.

## Running

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost imbalanced-learn tensorflow torch
jupyter notebook
```

Paths inside the notebooks are absolute and point at the original machine, so edit the data
locations at the top of each notebook before running.

## Results

The saved outputs in `Model.ipynb` show training and validation accuracy of about 92-93% within the
first epochs for the neural networks. One saved classification report shows very uneven recall across
classes (some attack classes near zero), so overall accuracy alone is not a reliable summary of how
well the attacks are detected. Treat the notebook as exploratory work, not a finished detector.
