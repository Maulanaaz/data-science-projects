# 📁 Project Directory

Below is an overview of all standalone data‑science & machine‑learning projects contained in this repository.  
Each sub‑folder includes a dedicated README, Jupyter notebooks, and (if needed) scripts for data download and model inference.

| Status | Project | What it does | Main techniques |
|--------|---------|--------------|-----------------|
| ✅ | [Flower Image Classification](./flower_images_classification) | Classify 14 flower species from photos | Transfer learning – MobileNetV2, CNN, TensorFlow |
| 🚧 | [Coronary Heart Disease Prediction](./coronary_heart_disease) | Predict CHD risk from patient records | EDA, XGBoost, ROC‑AUC, SHAP |
| 🚧 | [AG News Topic Classification](./ag_news_classification) | Categorise news headlines (politics, sports, tech, biz) | NLP preprocessing, TF‑IDF, Logistic Regression |
| 🚧 | [Book Recommender System](./book_recommender) | Recommend books based on user ratings | Collaborative filtering, cosine similarity |

> **Legend**  
> ✅ Finished & maintained 🚧 In progress 🧪 Experimental

---

### How to run a project

```bash
cd projects/<project_name>
pip install -r requirements.txt
jupyter notebook
