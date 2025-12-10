# SMS Spam Detection (Natural Language Processing)

## 🎯 Project Goal

The primary goal of this project was to build a robust classification model to automatically identify SMS messages as either **'ham'** (legitimate) or **'spam'** (unwanted). For a business application, the key focus was minimizing **False Positives** (flagging a ham message as spam), as this negatively impacts customer trust.

---

## 📊 Data & Pipeline Methodology

This project processes unstructured text data using a standard Natural Language Processing (NLP) pipeline.

### Data Source

* **Dataset:** SMS Spam Collection Dataset
* **Format Note:** The file is structured as a Tab-Separated Values (TSV) file, requiring the use of `sep='\t'` for correct loading.

### Text Preprocessing & Vectorization

The raw text data was transformed into numerical features using the following sequential steps:

1.  **Cleaning:** Messages were converted to lowercase and punctuation was removed.
2.  **Tokenization & Stop-word Removal:** Messages were broken down into individual words (tokens), and common English stop words (`is`, `the`, `a`) were filtered out.
3.  **Vectorization (Count-of-Words):** Messages were converted into a sparse matrix representation using `CountVectorizer`.
4.  **Weighting (TF-IDF):** Term Frequency-Inverse Document Frequency (`TfidfTransformer`) was applied to weigh words based on their relevance (giving higher weight to rare words common in spam, like "FREE" or "URGENT").

### Key Exploratory Data Analysis (EDA) Insight

* A notable difference was found in message length: **Spam messages are typically longer** (centered around 150 characters) compared to 'ham' messages (centered around 1-100 characters). This engineered `length` feature serves as a strong predictor.

---

## 💻 Modeling & Performance

The analysis compared a simple baseline model (Multinomial Naive Bayes) against a robust ensemble method (Random Forest), both integrated into a scikit-learn `Pipeline`.

### Final Performance Metrics (Spam Class)

| Model | Key Metric (Precision) | Recall | F1-Score |
| :--- | :--- | :--- | :--- |
| **Multinomial Naive Bayes (Baseline)** | 1.00 | 0.73 | 0.85 |
| **Random Forest (Final Model)** | **1.00** | **0.83** | **0.91** |

### Final Model Justification

The **Random Forest Classifier** was selected as the final model due to its superior performance.

1.  **High Precision Maintained:** Both models achieved **perfect Precision (1.00)**, meaning that **no legitimate customer messages were incorrectly flagged as spam** (zero False Positives)—a critical business requirement.
2.  **Increased Spam Capture (Recall):** The Random Forest model demonstrated a **significantly higher Recall** (**0.83** compared to the baseline's 0.73), ensuring that a larger percentage of the *actual* spam messages were correctly caught.

* **High Precision** is critical for this problem because it minimizes **False Positives** (flagging a user's legitimate message as spam), which is crucial for retaining user satisfaction.

---

## 💡 Business Recommendations

1.  **Prioritize Feature Keywords:** The model confirmed that terms like "FREE," "WINNER," "CLAIM," and high message length are the strongest predictors. Spam filters should prioritize blocking based on the presence and density of these commercial keywords.
2.  **Implement Filtering Pipeline:** The tested NLP pipeline (`CountVectorizer` $\rightarrow$ `TfidfTransformer` $\rightarrow$ `RandomForestClassifier`) is efficient and should be deployed in the production spam detection system.
3.  **Future Work (Oversampling):** Given the data imbalance (many ham messages vs. few spam messages), future work should explore techniques like SMOTE or class weighting during model training to potentially further improve the model's ability to catch rare spam messages.

---

## 🛠️ Requirements & Execution

This project requires standard Python data science libraries.

* `pandas`, `numpy`, `matplotlib`, `seaborn`
* `scikit-learn`
* `nltk` (Natural Language Toolkit)

### Execution Note

The notebook uses `random_state=42` for data splitting and model initialization to ensure full reproducibility of the reported metrics.
