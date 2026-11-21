# Modèles

| Fichier | Contenu |
|---|---|
| `tfidf_vectorizer_seen.pkl` + `logistic_regression_seen.pkl` | **Baseline officielle** (notebook 04), entraînée sur `train_seen.csv` (sans jailbreak). Vectorizer TF-IDF en n-grammes de caractères (3-5), régression logistique `class_weight="balanced"`. |
| `tfidf_vectorizer.pkl` + `logistic_regression.pkl` | Ancienne baseline entraînée sur `train_clean.csv` (toutes familles). Conservée pour comparaison. |

Chargement :
```python
import joblib
vec = joblib.load("models/tfidf_vectorizer_seen.pkl")
clf = joblib.load("models/logistic_regression_seen.pkl")
proba = clf.predict_proba(vec.transform(["Ignore previous instructions"]))[:, 1]
```
