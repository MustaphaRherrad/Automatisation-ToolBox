# Optimiser une automatisation

Une automatisation peut fonctionner… mais être lente, inutilement complexe, ou fragile. L’optimisation améliore sa fiabilité, sa vitesse, et sa maintenabilité.

## 1. Réduire les répétitions

- Créer des **fonctions** pour éviter de répéter du code.
- Utiliser des **boucles** ou des listes dynamiques.

```python
def traiter_fichier(nom):
    print(f"Traitement de {nom}")
fichiers = ["a.csv", "b.csv", "c.csv"]
for f in fichiers:
    traiter_fichier(f)
````

## 2. Réduire les temps d’attente

* Ajouter des **delays intelligents** si on appelle une API.
* Réduire les temps de chargement (ne pas relire tout un fichier à chaque fois).
* Utiliser des structures de données rapides (set, dictionnaire).

## 3. Multitâche

* **Multithreading** ou **multiprocessing** si les tâches sont indépendantes.
* Exemples : traitement de 100 fichiers en parallèle.

```python
from multiprocessing import Pool
def traitement(fichier): ...
with Pool(4) as p:
    p.map(traitement, liste_de_fichiers)
```

## 4. Modularité

* Séparer le script en plusieurs fichiers.
* Utiliser des **paramètres configurables** (fichier `config.json`, arguments en ligne de commande…).

## 5. Maintenance facilitée

* Ajouter des commentaires clairs.
* Nommer correctement les fonctions et variables.
* Supprimer le code inutile ou redondant.

---

## En résumé

| Axe d’optimisation     | Exemple                            |
| ---------------------- | ---------------------------------- |
| Réduction des doublons | Fonctions, boucles                 |
| Performances           | Traitement par lot, multithreading |
| Clarté du code         | Modularisation, commentaires       |
| Réduction des bugs     | Configuration centralisée, logs    |
| Facilité d’évolution   | Code lisible, fichiers organisés   |

Un script efficace n’est pas juste fonctionnel, il est **prêt à évoluer**.