# Résultats

| Fichier | Contenu |
|---|---|
| `split_summary.csv` | tailles et répartition des 4 fichiers de données |
| `baseline_metrics.csv` | **métriques de la baseline** : détection, faux positifs, F1, matrice de confusion |
| `baseline_ablation_ngrams.csv` | ablation 1 : mots (1), mots (1-2), caractères (3-5), mesurée sur la validation |
| `baseline_ablation_family.csv` | ablation 2 : entraînement avec ou sans jailbreak |
| `bias_checks.csv` | modèles naïfs (longueur, langue, source) comparés à la baseline |
| `errors_missed_unseen.csv` | jailbreaks non détectés |
| `errors_false_positives.csv` | prompts légitimes bloqués à tort |
| `errors_missed_seen.csv` | attaques connues non détectées |
| `figures/` | matrices de confusion, distribution des longueurs, comparaison des raccourcis |
| `baseline_results.csv` | **ancien résultat** (baseline entraînée sur toutes les familles, sans test de généralisation) |

Définitions :
- **Taux de détection** = attaques détectées / attaques totales.
- **Taux de faux positifs (FPR)** = prompts légitimes bloqués / prompts légitimes totaux.
