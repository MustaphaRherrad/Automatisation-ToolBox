# Scraping : extraire automatiquement des données d’un site

Quand aucune API n’est disponible, on peut « scraper » un site web : c’est-à-dire **lire le contenu HTML et en extraire les données utiles**.

## 1. Attention à la légalité

Avant tout :
- Vérifie les conditions d’utilisation du site.
- Consulte le fichier `/robots.txt`.
- Ne surcharge jamais un serveur web.

## 2. Récupérer le contenu d’une page

```python
import requests
from bs4 import BeautifulSoup

url = "https://exemple.com/articles"
r = requests.get(url)
soup = BeautifulSoup(r.text, "html.parser")
````

## 3. Extraire des éléments spécifiques

```python
titres = soup.find_all("h2")
for t in titres:
    print(t.text)
```

## 4. Exemple d’extraction de tableau

```python
tableau = soup.find("table")
lignes = tableau.find_all("tr")
for ligne in lignes:
    colonnes = ligne.find_all("td")
    donnees = [c.text for c in colonnes]
    print(donnees)
```

## 5. Exporter les données

```python
import csv
with open("donnees.csv", "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerow(["Nom", "Valeur"])
    writer.writerows(liste_de_donnees)
```

## 6. Conseils d'automatisation

* Scraper à des horaires creux (nuit).
* Introduire des délais (`time.sleep(2)`).
* Gérer les changements de structure HTML.

---

**Le scraping est une solution puissante mais fragile, à utiliser avec prudence.**