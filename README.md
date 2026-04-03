# iot-attack-prediction

A data mining project that classifies cyberattack types on IoT network traffic using a Decision Tree Classifier, deployed as an interactive Gradio web interface. Built on the RT-IoT2022 dataset from the UCI Machine Learning Repository.

---

## Overview

IoT devices are increasingly targeted by sophisticated cyber threats. This project trains a classification model on real-world IoT network traffic data to predict the type of attack occurring — before it causes damage — using early network flow indicators.

The model achieves **~100% weighted accuracy** and a **macro F1-score of 0.97** across 12 attack classes, including rare minority classes like `Metasploit_Brute_Force_SSH` and `NMAP_FIN_SCAN`.

---

## Project Structure

```
iot-attack-prediction/
├── notebook/
│   └── iot_attack_prediction.ipynb
├── model/
│   └── model.pkl
├── report/
│   └── iot_detection.pdf
└── README.md
```

---

## Dataset

**RT-IoT2022** — UCI Machine Learning Repository  
[https://archive.ics.uci.edu/dataset/942/rt-iot2022](https://archive.ics.uci.edu/dataset/942/rt-iot2022)

| Property | Detail |
|---|---|
| Rows | 123,117 |
| Features | 85 |
| Target variable | `attack_type` (12 classes) |
| IoT devices | ThingSpeak-LED, Wipro-Bulb, MQTT-Temp Sensor |
| Monitoring tools | Zeek, Flowmeter |

**Attack classes covered:**

| Category | Attack Types |
|---|---|
| Network floods | `DOS_SYN_Hping`, `DDOS_Slowloris` |
| Reconnaissance | `NMAP_TCP_scan`, `NMAP_UDP_SCAN`, `NMAP_FIN_SCAN`, `NMAP_OS_DETECTION`, `NMAP_XMAS_TREE_SCAN` |
| Brute force | `Metasploit_Brute_Force_SSH` |
| IoT protocol attacks | `MQTT_Publish`, `ARP_poisioning`, `Thing_Speak`, `Wipro_bulb` |

---

## Methodology

Follows the **CRISP-DM** framework:

1. **Business Understanding** — Identify early indicators of IoT cyberattacks in real time
2. **Data Understanding** — Exploratory analysis: class distribution, feature correlation heatmaps, feature importance via Random Forest and XGBoost
3. **Data Preparation** — Missing value removal, duplicate cleanup, column name standardization, label encoding of categorical features
4. **Modeling** — Decision Tree Classifier (scikit-learn, `random_state=42`, stratified 70/30 split)
5. **Evaluation** — Classification report + dual confusion matrix analysis (full dataset and DOS_SYN_Hping-excluded for minority class visibility)
6. **Deployment** — Model serialized with `joblib`, served via Gradio interface on Google Colab with a public shareable URL

---

## Results

| Metric | Score |
|---|---|
| Weighted accuracy | ~1.00 |
| Macro F1-score | 0.97 |
| Macro precision | 0.97 |
| Macro recall | 0.97 |

Most misclassifications occurred between rare attack types with similar network behavior — expected in real-world intrusion detection scenarios. A second confusion matrix was generated excluding the dominant `DOS_SYN_Hping` class to properly evaluate minority class performance.

**Top predictive features** (Random Forest + XGBoost): `fwd_pkts_payload.avg`, `id.resp_p`, `fwd_pkts_payload.min`, `service`

---

## Deployment

The trained model is deployed as a **Gradio web interface** hosted on Google Colab. Users input:

- Protocol (`proto`) — dropdown
- Service type — dropdown
- Flow duration — numeric
- Total forward packets (`fwd_pkts_tot`) — numeric

The interface returns a predicted attack type in real time. The same `.pkl` model and preprocessing pipeline can be adapted for permanent hosting on Hugging Face Spaces or Flask/FastAPI.

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/<your-username>/iot-attack-prediction.git
cd iot-attack-prediction

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# Open the notebook
jupyter notebook notebook/iot_attack_prediction.ipynb
```

All dependencies are installed via `pip install` cells inside the notebook. The notebook is designed to run on **Google Colab** — the Gradio interface requires Colab's `share=True` to generate a public URL.

---

## Tools and Libraries

- **Python 3.11**
- `scikit-learn` — Decision Tree, train/test split, classification metrics
- `xgboost` — feature importance analysis
- `gradio` — interactive prediction interface
- `joblib` — model serialization
- `pandas`, `numpy` — data manipulation
- `matplotlib`, `seaborn` — visualization

---

## Report

The `report/` folder contains the full study paper covering all CRISP-DM phases, dataset description, feature importance analysis, model evaluation, and deployment discussion.

---

## Authors

Mining Mavericks Group — Mapúa University, School of Information Technology

Rob Eugene Dequiñon · Jan Christian Diesta · Dustin Joaquin B. Granada · Sean Jared R. Santos · Yuan Benedict Suarez