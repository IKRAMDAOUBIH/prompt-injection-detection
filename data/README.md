# Données

## Source
- **Dataset** : `dmilush/shieldlm-prompt-injection` (Hugging Face), qui agrège plusieurs jeux publics : `alespalla/chatbot-instruction-prompts`, `spml/chatbot-prompt-injection`, `safeguard/prompt-injection`, `harelix/mixed-techniques-2024`, `trustailab/in-the-wild-jailbreak-prompts`, `yanismiraoui/prompt-injections`, `deepset/prompt-injections`, `jackhhao/jailbreak-classification`, `jailbreakbench/jbb-behaviors`, `injecagent/*`.
- **Licence** : à vérifier sur la page Hugging Face du dataset et de chacune de ses sources avant le rendu. *(À COMPLÉTER)*
- **Langue** : anglais à 99 % environ. Les autres langues (fr, es, de, it, pt, ro, ca) représentent moins de 1 % et sont **toutes malveillantes**.
- **Exemples synthétiques** : aucun n'a été ajouté.

## Colonnes
| Colonne | Signification |
|---|---|
| `id` | identifiant du prompt (certains identifiants sont répétés, voir notebook 02) |
| `text` | le prompt |
| `label_binary` | **0 = légitime, 1 = malveillant** |
| `family` / `label_category` | `benign`, `direct_injection`, `indirect_injection`, `jailbreak` |
| `source` | sous-jeu d'origine |
| `language` | langue détectée |
| `split` | ensemble d'origine (train / validation / test) |

## Nettoyage (notebook 02)
- Aucun texte vide, aucun doublon exact de texte.
- Les identifiants répétés entre ensembles (30) sont tous placés dans train, pour qu'un même groupe ne soit pas partagé entre train et test.

## Familles d'attaques
Les familles sont celles fournies par le dataset (colonne `label_category`). Elles n'ont pas été recréées par règles de mots-clés.

| Famille | Description | Exemple de forme |
|---|---|---|
| `direct_injection` | consigne malveillante écrite directement par l'utilisateur | « Oublie tes règles et… » |
| `indirect_injection` | consigne cachée dans des données (JSON, email, agenda…) | champ `short_bio` contenant une instruction |
| `jailbreak` | longs scénarios de jeu de rôle pour contourner les règles | « Tu es DAN, une IA sans limites… » |

**Famille retirée de l'entraînement : `jailbreak`** (1 018 exemples : 712 train, 153 validation, 153 test d'origine).

## Fichiers produits (notebook 03)
| Fichier | Lignes | Légitimes | Malveillants | Familles |
|---|---|---|---|---|
| `train_seen.csv` | 37 219 | 24 638 | 12 581 | benign, direct, indirect |
| `val_seen.csv` | 7 961 | 5 279 | 2 682 | benign, direct, indirect |
| `test_seen.csv` | 7 964 | 5 280 | 2 684 | benign, direct, indirect |
| `test_unseen_family.csv` | 6 298 | 5 280 | 1 018 | benign, jailbreak |

`test_unseen_family.csv` réutilise les 5 280 prompts légitimes de `test_seen` (même ensemble de test d'origine). Ils ne sont pas dans le train.


## Limites connues
1. **Biais de langue** : aucun prompt légitime non anglais, donc les faux positifs sur du français légitime ne sont pas mesurables.
2. **Biais de source** : presque tous les jailbreaks viennent de `trustailab/in-the-wild-jailbreak-prompts`. « Famille retirée » = aussi « source retirée ».
3. **Biais de longueur** : longueur médiane de 68 caractères pour les légitimes contre 323 pour les malveillants (voir notebook 05).
4. **Amélioration possible (non réalisée, à décider en équipe)** : ajouter des prompts légitimes en français, générés par un LLM, en documentant le LLM, le prompt et le nombre d'exemples.
