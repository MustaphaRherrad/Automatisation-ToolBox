# Traitement par lot : automatiser à grande échelle

Quand il faut répéter la même tâche sur des **dizaines, centaines ou milliers de fichiers**, on parle de traitement par lot.

## 1. Exemple de scénario

- Convertir 500 fichiers `.xlsx` en `.csv`.
- Renommer 200 images.
- Appliquer un filtre à tous les fichiers `.txt` dans un dossier.

## 2. Parcourir un dossier

```python
import os

for fichier in os.listdir("donnees"):
    if fichier.endswith(".xlsx"):
        print(f"À traiter : {fichier}")
````

## 3. Application d’un traitement

```python
import pandas as pd
import os

for fichier in os.listdir("donnees"):
    if fichier.endswith(".xlsx"):
        df = pd.read_excel(f"donnees/{fichier}")
        df.to_csv(f"resultats/{fichier.replace('.xlsx', '.csv')}", index=False)
```

## 4. Parallélisation (bonus)

Pour gagner du temps, on peut utiliser le **traitement parallèle** (multiprocessing).

```python
from multiprocessing import Pool

def convertir(fichier):
    df = pd.read_excel(f"donnees/{fichier}")
    df.to_csv(f"resultats/{fichier.replace('.xlsx', '.csv')}", index=False)

fichiers = [f for f in os.listdir("donnees") if f.endswith(".xlsx")]

with Pool(4) as p:
    p.map(convertir, fichiers)
```

## 5. Surveillance du traitement

* Afficher une barre de progression (via `tqdm`).
* Enregistrer les erreurs dans un fichier `log`.

---

## En résumé

| Étape                   | Description                         |
| ----------------------- | ----------------------------------- |
| Boucle sur les fichiers | Identifier les fichiers à traiter   |
| Traitement individuel   | Nettoyage, conversion, renommage…   |
| Gestion d’erreurs       | Continuer même si un fichier échoue |
| Optimisation            | Parallélisation, tri, sélection     |

Le traitement par lot est la **colonne vertébrale de l'automatisation à grande échelle**