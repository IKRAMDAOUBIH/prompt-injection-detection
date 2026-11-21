# Détection de prompt injection et jailbreak

Projet du module Deep Learning (filière ADIA, FSA Agadir). Classifieur binaire qui décide si un prompt est **légitime (0)** ou **malveillant (1)** (prompt injection ou jailbreak).

- **Baseline** : TF-IDF + régression logistique.
- **Modèle deep learning** : DistilBERT ou BERT-mini (Membre 2).
- **Généralisation** : une famille d'attaques (`jailbreak`) est retirée de l'entraînement et utilisée uniquement au test.
- **Langue des données** : principalement l'anglais (voir `data/README.md` pour les limites).

## Organisation du dépôt

```
data/
  raw/                      données brutes (vide, le dataset est téléchargé depuis Hugging Face)
  processed/
    train_clean.csv         (notebook 02) données nettoyées, toutes familles
    validation_clean.csv
    test_clean.csv
    train_seen.csv          (notebook 03) SANS la famille retirée -> à utiliser pour entraîner
    val_seen.csv            SANS la famille retirée -> à utiliser pour régler
    test_seen.csv           SANS la famille retirée -> test sur attaques connues
    test_unseen_family.csv  famille retirée + prompts légitimes -> test de généralisation
  README.md                 description détaillée des données
notebooks/person1/          notebooks du Membre 1 (01 à 05)
models/                     modèles de la baseline (.pkl)
results/                    métriques (.csv), erreurs (.csv), figures
```

## Ordre d'exécution des notebooks (Membre 1)

1. `01_data_exploration.ipynb` : exploration du dataset Hugging Face.
2. `02_preprocessing.ipynb` : vérifications et nettoyage, produit `*_clean.csv`.
3. `03_unseen_family_split.ipynb` : produit `train_seen / val_seen / test_seen / test_unseen_family`.
4. `04_baseline_tfidf_lr.ipynb` : baseline, ablations, analyse d'erreurs.
5. `05_bias_checks.ipynb` : vérification des raccourcis (longueur, langue, source).

Les notebooks 03 à 05 s'exécutent depuis n'importe quel dossier du dépôt (ils remontent seuls à la racine).

## Installation

```
pip install -r requirements.txt
```

## Résultats de la baseline (résumé)

| Ensemble | Taux de détection | Taux de faux positifs | F1 |
|---|---|---|---|
| validation (seen) | 96,3 % | 1,4 % | 0,967 |
| test_seen | 96,8 % | 1,3 % | 0,971 |
| **test_unseen_family** (jailbreak jamais vu) | **88,3 %** | 1,3 % | 0,905 |

Détails, ablations et analyse d'erreurs : dossier `results/`.

