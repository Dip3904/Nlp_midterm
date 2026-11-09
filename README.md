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

#### 3. Install Required Dependencies
```bash
pip install -r requirements.txt
```


## Dataset Citation & Licence
- **Source:** Almeida, T. A., Hidalgo, J. M. G., & Yamakami, A. (2011). *Contributions to the Study of SMS Spam Filtering: New Collection and Results*. Proceedings of the 2011 ACM Symposium on Document Engineering (DocEng '11).
- **Repository:** [UCI Machine Learning Repository — SMS Spam Collection](https://archive.ics.uci.edu/dataset/228/sms+spam+collection)
- **Licence:** Creative Commons Attribution 4.0 International (**CC BY 4.0**).
