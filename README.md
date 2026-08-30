# INTERSHIP-REPO-AI-PHISHING-DETECTION

# 🛡️ AI Phishing Detection Using Machine Learning

An AI-powered phishing URL detection system that uses **Machine Learning** to classify URLs as **Legitimate** or **Phishing**.

The project trains multiple classification models and selects **Random Forest** as the production model. It uses a fixed set of **22 URL, domain, and HTML/JavaScript-related features** to identify characteristics associated with phishing websites.

---

## 📌 Project Overview

Phishing attacks use deceptive websites and URLs to trick users into revealing sensitive information such as passwords, banking credentials, and personal data.

This project aims to detect potentially malicious URLs using a machine-learning-based classification approach.

The system follows this pipeline:

```text
phishing.csv
      │
      ▼
Data Acquisition
      │
      ▼
Data Cleaning & Preprocessing
      │
      ▼
22 Feature Selection
      │
      ▼
Exploratory Data Analysis
      │
      ▼
Train / Test Split
      │
      ▼
Model Training
 ┌────┼───────────────────┐
 ▼    ▼                   ▼
Logistic Regression   Decision Tree   Random Forest
                                      │
                                      ▼
                              Model Evaluation
                                      │
                                      ▼
                              Save Trained Model
                                      │
                                      ▼
                             Real-Time URL Prediction
```

---

## 🎯 Objectives

* Detect phishing URLs using machine learning.
* Extract meaningful characteristics from URLs and domains.
* Use a fixed set of **22 features** for model training.
* Compare multiple classification algorithms.
* Select the best-performing model.
* Save the trained Random Forest model for later predictions.
* Provide a real-time URL prediction mechanism.
* Generate a phishing probability/confidence score.

---

## 🧠 Why "AI Phishing Detection"?

The project is called **AI Phishing Detection** because it uses a machine-learning classification model to automatically learn patterns from labelled phishing and legitimate URL data.

Instead of relying only on manually written rules, the trained model evaluates multiple features simultaneously and predicts whether a URL belongs to the **Phishing** or **Legitimate** class.

---

## 📊 Dataset

The project uses the main:

```text
phishing.csv
```

The notebook downloads the dataset automatically from the project's GitHub repository and also provides a manual upload fallback if automatic downloading fails.

### Dataset Statistics

| Property                  |   Value |
| ------------------------- | ------: |
| Original records          | 235,795 |
| Original features/columns |      56 |
| Duplicate URLs            |     425 |
| Records after cleaning    | 235,370 |
| Features used for ML      |      22 |
| Training split            |     80% |
| Testing split             |     20% |
| Random state              |      42 |

The original label distribution contains:

* **134,850** legitimate records
* **100,945** phishing records

After duplicate removal, the dataset contains:

* **134,850** legitimate records
* **100,520** phishing records

---

## 🔍 Feature Engineering

The project uses **22 features**, divided into three groups.

### 1. Address Bar Features

These features examine characteristics directly visible in the URL.

```text
URLLength
IsDomainIP
HasObfuscation
NoOfSubDomain
IsHTTPS
URLSimilarityIndex
NoOfEqualsInURL
NoOfQMarkInURL
SpacialCharRatioInURL
```

### 2. Domain Features

These features analyze properties of the domain and TLD.

```text
DomainLength
TLDLegitimateProb
CharContinuationRate
URLCharProb
TLDLength
```

### 3. HTML / JavaScript Features

These features represent client-side characteristics of the website.

```text
NoOfiFrame
NoOfPopup
HasHiddenFields
NoOfSelfRedirect
HasExternalFormSubmit
HasSubmitButton
HasPasswordField
HasSocialNet
```

The 22 features are combined into a single feature vector used by the machine-learning models.

---

## 🧹 Data Preprocessing

Before training, the dataset goes through several preprocessing steps:

1. Select required columns.
2. Remove missing URL and label records.
3. Remove duplicate URLs.
4. Normalize URL strings.
5. Convert labels into the project's ML convention:

   * `0` → Legitimate
   * `1` → Phishing
6. Handle missing numerical feature values using median imputation.
7. Reset the dataframe index.

After preprocessing:

```text
Rows after cleaning : 235370
Feature columns kept: 22
```

---

## 🤖 Machine Learning Models

Three classification algorithms are trained and compared.

### Logistic Regression

Used as a linear baseline model.

### Decision Tree

Used to evaluate a tree-based classification approach.

### Random Forest

An ensemble of decision trees.

The Random Forest configuration used in the notebook is:

```python
RandomForestClassifier(
    n_estimators=200,
    random_state=42,
    n_jobs=-1
)
```

Random Forest is selected as the production candidate based on its performance on the test data.

---

## 📈 Model Evaluation

The models are evaluated using:

* Accuracy
* Precision
* Recall
* F1 Score
* ROC-AUC

The notebook also generates a detailed classification report for the Random Forest model.

The project reports a final accuracy of approximately:

```text
99.99%
```

> **Note:** Model performance can vary if the dataset, preprocessing, feature selection, or train/test split changes.

---

## 🧪 Train/Test Split

The dataset is divided using:

```python
train_test_split(
    X,
    y,
    test_size=0.20,
    random_state=42,
    stratify=y
)
```

This results in an **80/20 training and testing split**, while stratification preserves the class distribution.

---

## 💾 Saved Model

After training, the Random Forest model and feature names are saved using `joblib`.

### Model

```text
phishing_rf_model.pkl
```

### Feature Names

```text
phishing_feature_names.pkl
```

These files allow the trained model to be loaded later without retraining it.

---

## ⚡ Real-Time URL Prediction

The project includes a real-time prediction engine that accepts a URL and extracts the same 22-feature vector used during training.

Example:

```python
predict_url("https://www.google.com")
```

The system returns:

```text
URL      : https://www.google.com
Result   : LEGITIMATE
Confidence (phishing probability): ...
```

For a suspicious URL:

```python
predict_url("http://paypal-account-update.tk/confirm")
```

the system can classify it as:

```text
Result   : PHISHING
```

The prediction engine calculates URL and domain characteristics such as:

* URL length
* IP-based domain detection
* URL obfuscation
* Number of subdomains
* HTTPS usage
* URL similarity
* Query parameters
* Special-character ratio
* Domain length
* TLD characteristics
* URL entropy-related information

The notebook contains a shared feature-extraction function so that the real-time prediction process uses the same feature ordering as the trained model.

---

## 🖥️ Standalone Prediction Script

The notebook also generates:

```text
predict.py
```

The standalone script loads:

```text
phishing_rf_model.pkl
phishing_feature_names.pkl
```

and provides URL prediction without requiring the complete training notebook to be executed again.

---

## 📁 Suggested Repository Structure

```text
AI-PHISHING-DETECTION/
│
├── data/
│   └── phishing.csv
│
├── models/
│   ├── phishing_rf_model.pkl
│   └── phishing_feature_names.pkl
│
├── notebooks/
│   └── final.ipynb
│
├── predict.py
│
├── app.py
│
├── requirements.txt
│
└── README.md
```

> The exact repository structure may differ depending on how the project files are organized.

---

## ⚙️ Technologies Used

| Technology   | Purpose                        |
| ------------ | ------------------------------ |
| Python       | Core programming language      |
| Pandas       | Data manipulation              |
| NumPy        | Numerical operations           |
| Scikit-learn | Machine learning               |
| Matplotlib   | Data visualization             |
| Joblib       | Model serialization            |
| Streamlit    | Application/UI support         |
| Google Colab | Notebook execution environment |

The notebook installs these primary dependencies directly.

---

## 🚀 Installation

Clone the repository:

```bash
git clone https://github.com/mohitsain12/INTERSHIP-REPO-AI-PHISHING-DETECTION.git
```

Move into the project directory:

```bash
cd INTERSHIP-REPO-AI-PHISHING-DETECTION
```

Install the dependencies:

```bash
pip install pandas numpy scikit-learn matplotlib joblib streamlit
```

---

## ▶️ Running the Notebook

The main implementation is provided in:

```text
final.ipynb
```

The notebook can be opened using **Google Colab** or Jupyter Notebook.

### Google Colab

Upload/open:

```text
final.ipynb
```

Then execute the notebook cells sequentially.

The notebook automatically attempts to download the dataset. If that fails, it provides a manual upload option for `phishing.csv`.

---

## 🔎 Running URL Prediction

Once the trained model files are available:

```bash
python predict.py
```

The prediction script loads the saved Random Forest model and performs predictions on example URLs.

You can also use the prediction function directly:

```python
from predict import predict_url

predict_url("https://example.com")
```

---

## ⚠️ Important Limitation

The real-time prediction implementation extracts URL and domain features directly from the URL string.

Some HTML/JavaScript features require access to the actual webpage. In the current implementation, these features are therefore assigned default values when the page itself is not fetched.

This means the real-time predictor should be considered a **URL-based detection system**, rather than a complete webpage crawler/analyzer.

The notebook explicitly documents this limitation in its real-time feature extraction implementation.

---

## 🔐 Security Note

This project is intended for:

* Educational purposes
* Machine-learning research
* Cybersecurity learning
* Phishing detection experimentation
* Internship/project demonstration

The system should **not** be treated as a replacement for production-grade browser security, threat-intelligence systems, URL reputation services, or security gateways.

A machine-learning prediction is probabilistic and can produce false positives or false negatives.

---

## 🧪 Example Predictions

The notebook demonstrates predictions using both legitimate and suspicious URLs.

Example test URLs include:

```text
https://www.google.com
https://github.com
http://192.168.1.10/login/verify?user=admin
http://secure-account-login.xyz/verify-password=123
http://paypal-account-update.tk/confirm
```

It also demonstrates a typo-squatting-style URL:

```text
https://patpal/online/registration.com
```

which was classified as:

```text
PHISHING
```

with a reported phishing probability of **97%** in the notebook output.

---

## 🔮 Future Improvements

Possible improvements include:

* 🌐 Live webpage HTML/JavaScript feature extraction
* 🔍 Real-time URL reputation checking
* 🧠 Hyperparameter optimization
* 📊 Additional ML algorithms
* ⚖️ Cross-validation
* 📈 ROC and Precision-Recall visualization
* 🖥️ Improved Streamlit dashboard
* 🔄 Automated model retraining
* 🛡️ Browser-extension integration
* 📡 Integration with threat-intelligence feeds
* 🧪 Testing on completely unseen datasets

---

## 👨‍💻 Author

**Mohit Sain**
AI / Machine Learning + Cybersecurity Internship Project

---
## ⭐ Project Highlights

```text
🛡️ AI-Based Phishing Detection
🤖 Random Forest Classification
📊 22 Selected Features
🔎 URL & Domain Analysis
📈 Model Benchmarking
💾 Saved ML Model
⚡ Real-Time URL Prediction
🐍 Python + Scikit-learn
```

---
## 📜 Disclaimer

This project is developed for educational and research purposes. Predictions generated by the model should not be considered absolute proof that a website is safe or malicious. Always verify suspicious URLs using appropriate security tools and trusted sources.
---

