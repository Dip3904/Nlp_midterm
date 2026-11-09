# Automated SMS Spam Filtering — NLP Midterm Project (CSC4233)

An end-to-end comparative study of classical Natural Language Processing (NLP) preprocessing pipelines, text representations (**Bag of Words**, **TF-IDF N-grams**, and **Skip-Gram Word2Vec Embeddings**), and machine learning classifiers (**Majority Baseline**, **Multinomial Naive Bayes**, **Logistic Regression**, **Linear SVM**, plus optional **Gaussian Naive Bayes** and **Feedforward MLP**) on the **UCI SMS Spam Collection** corpus ($N = 5,574$).

---

## Repository Structure

```text
├── NLP_Midterm_Project_CSC4233.ipynb   # Self-contained Colab/Jupyter Notebook (all outputs saved)
├── dataset/
│   └── sms_spam_collection.csv         # Backup dataset (5,574 labelled SMS messages)
├── figures/                            # Generated 300-DPI visualizations (Figures 1–5)
│   ├── fig1_class_distribution.png
│   ├── fig2_document_length_stats.png
│   ├── fig3_top_words_before_after.png
│   ├── fig4_confusion_matrix.png
│   └── fig5_informative_features_nb.png
├── build_and_execute_project.py        # Automated pipeline runner & notebook builder
├── results_summary.json                # Exported evaluation metrics & error analysis tables
└── requirements.txt                    # Python package dependencies
```

---

## How to Run the Project

### Option 1: Run on Google Colab (Recommended — Zero Setup)
1. Upload [`NLP_Midterm_Project_CSC4233.ipynb`](./NLP_Midterm_Project_CSC4233.ipynb) to [Google Colab](https://colab.research.google.com/) (or open it directly from GitHub in Colab).
2. Click **`Runtime` $\to$ `Run all`**.
3. The notebook automatically installs any missing dependencies, downloads the dataset from the public URL (with fallback to this repository), fixes random seeds (`SEED = 42`), executes the full pipeline, and renders all tables and figures.

---

### Option 2: Run Locally Using a Virtual Environment (`.venv`)

#### 1. Clone the Repository
```bash
git clone https://github.com/lwLight01/Nlp_dip.git
cd Nlp_dip
```

#### 2. Create and Activate a Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux / macOS:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

#### 3. Install Required Dependencies
```bash
pip install -r requirements.txt
```

#### 4. Execute the Notebook or Pipeline

- **Open in VS Code / Jupyter Notebook:**
  Open `NLP_Midterm_Project_CSC4233.ipynb` and select the `.venv` Python kernel, then click **Run All**.

- **Or Rebuild & Execute Top-to-Bottom from Terminal:**
  ```powershell
  # Windows
  .\.venv\Scripts\python.exe build_and_execute_project.py

  # Linux / macOS
  python build_and_execute_project.py
  ```
  This script executes every notebook cell in order, regenerates all plots in `figures/`, updates `results_summary.json`, and saves the fully executed `NLP_Midterm_Project_CSC4233.ipynb` with embedded outputs.

---

## Summary of Results (`SEED = 42`, Stratified 70/15/15 Split)

- **Dataset Split:** `3,901` Train (70%) / `836` Validation (15%) / `837` Test (15%) — stratified at `13.4%` spam (`6.46 : 1` imbalance ratio).
- **Selected Preprocessing Pipeline (Validation Ablation):** Regex semantic normalization (`urltoken`, `moneytoken`, `phonetoken`) + negation-preserving stop-word removal + WordNet lemmatization (`Mean Val Macro-F1 = 0.9708`).

| Representation | Classifier | Tuned Hyperparameters | Val Macro-F1 | Test Accuracy | Test Macro-Prec | Test Macro-Rec | Test Macro-F1 |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| Baseline (Majority) | DummyClassifier | `strategy='most_frequent'` | 0.4641 | 0.8662 | 0.4331 | 0.5000 | 0.4641 |
| BoW (Count Vectors) | Multinomial Naive Bayes | `alpha=0.25, max_feat=3000` | 0.9720 | 0.9821 | 0.9538 | 0.9708 | 0.9621 |
| **BoW (Count Vectors)** | **Logistic Regression ★** | **`C=10.0, max_feat=3000`** | **0.9819** | **0.9928** | **0.9882** | **0.9808** | **0.9844** |
| BoW (Count Vectors) | Linear SVM | `C=0.05, max_feat=3000` | 0.9816 | 0.9940 | 0.9927 | 0.9815 | 0.9870 |
| TF-IDF (Unigram 1,1) | Multinomial Naive Bayes | `alpha=0.1, max_feat=3000` | 0.9764 | 0.9857 | 0.9760 | 0.9615 | 0.9686 |
| TF-IDF (Unigram 1,1) | Logistic Regression | `C=5.0, max_feat=3000` | 0.9742 | 0.9928 | 0.9811 | 0.9883 | 0.9847 |
| **TF-IDF (Unigram 1,1)** | **Linear SVM** | **`C=1.0, max_feat=3000`** | **0.9767** | **0.9940** | **0.9889** | **0.9852** | **0.9871** |
| TF-IDF (Uni+Bigram 1,2) | Multinomial Naive Bayes | `alpha=0.05, max_feat=3000` | 0.9686 | 0.9904 | 0.9866 | 0.9718 | 0.9791 |
| TF-IDF (Uni+Bigram 1,2) | Logistic Regression | `C=1.0, max_feat=3000` | 0.9744 | 0.9928 | 0.9845 | 0.9845 | 0.9845 |
| TF-IDF (Uni+Bigram 1,2) | Linear SVM | `C=0.05, max_feat=3000` | 0.9794 | 0.9928 | 0.9845 | 0.9845 | 0.9845 |
| Word2Vec (Avg 100d) | Logistic Regression | `C=5.0, weight=None` | 0.9557 | 0.9881 | 0.9851 | 0.9629 | 0.9736 |
| Word2Vec (Avg 100d) | Linear SVM | `C=0.5, weight=None` | 0.9557 | 0.9892 | 0.9859 | 0.9674 | 0.9764 |
| Word2Vec (Avg 100d) | Gaussian NB *(Optional)* | `var_smoothing=1e-9` | 0.8635 | 0.9534 | 0.8747 | 0.9580 | 0.9098 |
| Word2Vec (Avg 100d) | Feedforward MLP *(Optional)* | `hidden=(64,), alpha=0.001` | 0.9523 | 0.9845 | 0.9618 | 0.9722 | 0.9669 |

---

## Dataset Citation & Licence
- **Source:** Almeida, T. A., Hidalgo, J. M. G., & Yamakami, A. (2011). *Contributions to the Study of SMS Spam Filtering: New Collection and Results*. Proceedings of the 2011 ACM Symposium on Document Engineering (DocEng '11).
- **Repository:** [UCI Machine Learning Repository — SMS Spam Collection](https://archive.ics.uci.edu/dataset/228/sms+spam+collection)
- **Licence:** Creative Commons Attribution 4.0 International (**CC BY 4.0**).
