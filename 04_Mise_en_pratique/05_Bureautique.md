# 05 Bureautique

L'automatisation des tâches bureautiques est l'un des domaines où l'on peut rapidement voir des gains de productivité significatifs. De nombreuses entreprises, petites ou grandes, s'appuient encore lourdement sur des outils comme Excel, Word, et PowerPoint. Automatiser les interactions avec ces applications permet de transformer des heures de travail manuel répétitif en quelques secondes d'exécution de script.

### Pourquoi Automatiser la Bureautique ?

* **Saisie de Données :** Remplir automatiquement des formulaires Word ou des cellules Excel.
* **Traitement de Rapports :** Générer des rapports financiers, des synthèses, ou des présentations à partir de données brutes.
* **Extraction d'Informations :** Récupérer des données spécifiques depuis des documents (par exemple, des numéros de facture depuis un PDF, ou des listes depuis Excel).
* **Formatage et Standardisation :** Assurer la cohérence de la mise en page et du style à travers de multiples documents.
* **Fusion et Division :** Combiner plusieurs documents ou diviser un grand document en plusieurs petits.

L'objectif est de rendre vos documents "intelligents" et réactifs aux données, sans intervention humaine répétée.

### Concepts Clés

* **Fichiers Standards ouverts (OOXML) :** Les formats de fichiers modernes de Microsoft Office (`.docx`, `.xlsx`, `.pptx`) sont en fait des archives ZIP contenant des fichiers XML. Cela les rend manipulables par des programmes sans nécessiter l'installation d'Office.
* **Feuilles de Calcul (Excel) :** Organisées en lignes, colonnes et cellules. Les formules, les tableaux croisés dynamiques, et les graphiques peuvent également être automatisés.
* **Documents Texte (Word) :** Structurés en paragraphes, sections, tableaux, images. L'automatisation peut cibler le texte, les styles, et les éléments de mise en page.
* **Présentations (PowerPoint) :** Composées de diapositives, de zones de texte, d'images et de graphiques.

### Outils Python pour l'Automatisation Bureautique

Python dispose d'excellentes bibliothèques pour interagir avec les principaux formats bureautiques.

#### 1. `openpyxl` (Fichiers Excel `.xlsx`)

`openpyxl` est la bibliothèque de référence pour lire, écrire et modifier des fichiers Excel au format `.xlsx`. C'est incroyablement utile pour la gestion de données.

* **Points forts :**
    * Lecture et écriture de feuilles de calcul.
    * Accès aux cellules par coordonnées (`ws['A1']`) ou par ligne/colonne (`ws.cell(row=1, column=1)`).
    * Manipulation de styles, de formules, de tableaux et de graphiques.
* **Cas d'usage :**
    * Mettre à jour un rapport de ventes hebdomadaire en ajoutant de nouvelles données.
    * Extraire une liste de contacts d'une colonne spécifique.
    * Générer un fichier Excel avec des données calculées et des graphiques.

```mermaid
graph TD
    A[["Fichier Excel<br>(Source.xlsx)"]] --> B[[Script Python<br>openpyxl/pandas]]
    B --> C{L'utilisation de<br>DataFrame Pandas ?}
    C -->|Oui| D[["Traitement avec<br>pandas"]]
    C -->|Non| E[["Traitement direct<br>openpyxl"]]
    D & E --> F[["Données Transformées"]]
    F --> G[["Fichier Excel<br>(Output.xlsx)"]]

    %% Styles optionnels
    class A,G orange
    class B blue
    class C question
    class D,E green
    class F yellow
```
*Figure 12 : Flux d'automatisation Excel avec `openpyxl`*

#### 2. `python-docx` (Fichiers Word `.docx`)

`python-docx` vous permet de créer, modifier et générer des documents Word. Idéal pour les tâches de fusion et de génération de documents.

* **Points forts :**
    * Ajouter des paragraphes, des titres, des images, des tableaux.
    * Appliquer des styles.
    * Remplacer des placeholders de texte par des données dynamiques.
* **Cas d'usage :**
    * Générer des lettres personnalisées à partir d'une liste de clients.
    * Remplir un modèle de contrat avec des informations spécifiques.
    * Créer des rapports d'activité pré-remplis.

#### 3. `python-pptx` (Fichiers PowerPoint `.pptx`)

`python-pptx` est utilisé pour créer et modifier des présentations PowerPoint. Très utile pour automatiser des rapports visuels.

* **Points forts :**
    * Ajouter des diapositives, des zones de texte, des images.
    * Insérer des tableaux et des graphiques (via des données).
    * Appliquer des modèles de diapositives.
* **Cas d'usage :**
    * Générer des présentations de résultats mensuels avec des graphiques mis à jour.
    * Créer des diaporamas de formation à partir de contenu textuel.

#### 4. `PyPDF2` / `fitz` (PyMuPDF) (Fichiers PDF)

Pour les PDF, il y a deux approches principales : `PyPDF2` pour des manipulations de base et `fitz` (PyMuPDF) pour des extractions et modifications plus avancées.

* **PyPDF2 :** Fusionner, diviser, faire pivoter des pages, extraire du texte simple.
* **`fitz` (PyMuPDF) :** Extraire du texte avec des informations de mise en page, des images, des tableaux ; modifier des PDF (annotations, ajout de texte).

* **Points forts :**
    * Manipuler le format PDF, très courant pour les documents finaux.
    * Extraire des données non structurées de documents.
* **Cas d'usage :**
    * Extraire des numéros de facture de PDF pour traitement.
    * Fusionner des rapports PDF individuels en un seul document.
    * Diviser un manuel PDF en chapitres séparés.

### Intégration des LLM (Ollama)

Les LLM comme Ollama peuvent transformer la façon dont nous interagissons avec la bureautique, surtout pour le contenu textuel :

* **Rédaction Automatique :** Demander à Ollama de rédiger des paragraphes, des résumés ou des points clés pour vos documents Word/PowerPoint basés sur des données ou d'autres textes.
* **Analyse de Contenu :** Extraire du texte d'un document (via PyPDF2, python-docx), l'envoyer à Ollama pour une classification, un résumé, ou une extraction d'entités, puis insérer le résultat dans Excel ou un autre document.
* **Génération de Questions/Réponses :** Créer des Q&A pour des présentations ou des documents de formation.

**Exemple d'intégration :**
1.  Extraire des descriptions de produits d'un fichier Excel à l'aide d'openpyxl.
2.  Envoyer chaque description à Ollama pour générer une phrase de marketing percutante.
3.  Mettre à jour la colonne des descriptions marketing dans le même fichier Excel.

### Exemple Pratique : Mise à jour d'un Rapport Excel Simple

Nous allons créer un script qui ouvre un fichier Excel, ajoute une nouvelle ligne de données (par exemple, des ventes du jour), calcule un total et enregistre le fichier.

**Préparation :**
1.  Créez un dossier `bureautique_demo/`.
2.  Dans ce dossier, créez un fichier Excel nommé `rapport_ventes_mensuel.xlsx` avec le contenu suivant dans la feuille `Feuil1` (ou `Sheet1`) :

    | Date       | Produit | Quantité | Prix Unitaire | Total Ligne |
    | :--------- | :------ | :------- | :------------ | :---------- |
    | 2024-05-01 | A       | 10       | 50            | 500         |
    | 2024-05-02 | B       | 5        | 120           | 600         |
    | 2024-05-03 | A       | 15       | 50            | 750         |

3.  Installez `openpyxl` : `pip install openpyxl`

**Fichier `update_excel_report.py`** :

```python
# bureautique_demo/update_excel_report.py

from openpyxl import load_workbook
from openpyxl.utils import get_column_letter
from datetime import datetime

def update_sales_report(file_path, new_data_row):
    """
    Met à jour un rapport de ventes Excel en ajoutant une nouvelle ligne et en calculant le total.
    new_data_row: tuple (Produit, Quantité, Prix Unitaire)
    """
    try:
        # Charger le classeur existant
        workbook = load_workbook(filename=file_path)
        sheet = workbook.active # Ou workbook['NomDeLaFeuille']

        # Préparer la nouvelle ligne
        current_date = datetime.now().strftime("%Y-%m-%d")
        product, quantity, unit_price = new_data_row
        line_total = quantity * unit_price
        
        # Ajouter la nouvelle ligne
        sheet.append([current_date, product, quantity, unit_price, line_total])
        print(f"Nouvelle ligne ajoutée: {current_date}, {product}, {quantity}, {unit_price}, {line_total}")

        # Calculer le total général de la colonne 'Total Ligne'
        # On suppose que 'Total Ligne' est en colonne E (index 5)
        # Et les données commencent à la ligne 2
        total_column_index = 5 # Colonne E
        current_row = sheet.max_row
        
        # Trouver la première ligne de données (juste après l'en-tête)
        start_data_row = 2 

        # Créer une formule de somme pour la colonne 'Total Ligne'
        # Par exemple, si max_row est 5, la formule sera SUM(E2:E5)
        sum_formula = f"=SUM(E{start_data_row}:E{current_row})"
        
        # Écrire la formule dans une nouvelle cellule, par exemple, la ligne suivante sous la colonne Total Ligne
        # Et un label pour le total
        sheet.cell(row=current_row + 1, column=total_column_index -1).value = "TOTAL GÉNÉRAL :" # Colonne D
        sheet.cell(row=current_row + 1, column=total_column_index).value = sum_formula
        print(f"Formule de total ajoutée à la cellule {get_column_letter(total_column_index)}{current_row + 1}")


        # Sauvegarder le classeur
        workbook.save(filename=file_path)
        print(f"Rapport mis à jour et sauvegardé: {file_path}")
        return True

    except FileNotFoundError:
        print(f"Erreur: Le fichier '{file_path}' n'a pas été trouvé.")
        return False
    except Exception as e:
        print(f"Une erreur est survenue lors de la mise à jour du rapport: {e}")
        return False

# --- Utilisation de la fonction ---
REPORT_FILE = "bureautique_demo/rapport_ventes_mensuel.xlsx"

print("--- Test de mise à jour du rapport de ventes ---")
# Exemple d'ajout de données pour aujourd'hui
today_sales = ("Produit C", 7, 200) # (Produit, Quantité, Prix Unitaire)
update_sales_report(REPORT_FILE, today_sales)

# Vous pouvez exécuter cette fonction plusieurs fois avec différentes données
# pour voir le fichier Excel se mettre à jour
# update_sales_report(REPORT_FILE, ("Produit D", 12, 45))
```

**Pour exécuter cet exemple dans JupyterLab :**

1.  Créez un nouveau notebook `05_Bureautique.ipynb`.
2.  Créez le dossier `bureautique_demo/` et le fichier `rapport_ventes_mensuel.xlsx` comme décrit dans la section "Préparation".
3.  Collez le code de la fonction `update_sales_report` et son utilisation dans des cellules séparées.
4.  Exécutez la ou les cellules.
5.  Ouvrez le fichier `rapport_ventes_mensuel.xlsx` dans Excel pour voir les modifications.

### Conseils et Bonnes Pratiques

* **Fermez les Fichiers :** Assurez-vous que les fichiers bureautiques ne sont pas ouverts manuellement par Excel/Word lorsque votre script essaie de les modifier, sinon une erreur d'accès se produira.
* **Sauvegardes :** Faites toujours une copie de sauvegarde de vos fichiers originaux avant de lancer des scripts d'automatisation, surtout ceux qui modifient des données.
* **Robustesse :** Tenez compte des variations possibles dans la structure des documents (nouvelles colonnes, changements de noms de feuilles). Utilisez des recherches par en-tête plutôt que des index de colonnes fixes si possible.
* **Formatage :** Pour des rapports finaux visuellement attrayants, vous devrez probablement combiner l'automatisation des données avec un formatage (couleurs, bordures, polices) dans le script ou en utilisant des modèles pré-formatés.
* **Performance :** Pour de très gros fichiers ou des manipulations très complexes, envisagez des outils plus spécifiques ou l'intégration avec Pandas avant d'écrire dans Excel.

### Tableau Récapitulatif : Outils d'Automatisation Bureautique

| Logiciel Bureautique | Format Fichier | Outils Python Recommandés | Cas d'Usage Typique                         |
| :----------------- | :------------- | :------------------------ | :------------------------------------------ |
| **Excel** | `.xlsx`        | `openpyxl`, `pandas`      | Rapports financiers, gestion de données, calculs complexes |
| **Word** | `.docx`        | `python-docx`             | Génération de contrats, lettres personnalisées, rapports textuels |
| **PowerPoint** | `.pptx`        | `python-pptx`             | Création de présentations de résultats, diaporamas de formation |
| **PDF** | `.pdf`         | `PyPDF2`, `fitz` (PyMuPDF) | Extraction de texte/données, fusion/division de documents |
| **LLM (Ollama)** | Contenu Textuel | `ollama` (via API)        | Rédaction de contenu, résumé, analyse sémantique pour tout document |