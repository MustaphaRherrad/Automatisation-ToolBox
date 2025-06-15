# 01 Fichiers et dossiers
L'automatisation des tâches liées aux fichiers et aux dossiers est l'un des points de départ les plus accessibles et les plus utiles. Que vous soyez un développeur, un analyste de données, ou simplement quelqu'un qui gère beaucoup de documents, savoir automatiser ces opérations vous fera gagner un temps précieux et réduira les erreurs.

### Pourquoi Automatiser les Tâches Fichiers & Dossiers ?

Imaginez ces scénarios :

* **Organisation :** Vous recevez des centaines de fichiers par jour qui doivent être triés dans des dossiers spécifiques en fonction de leur nom, de leur date, ou de leur contenu.
* **Nettoyage :** Des dossiers s'encombrent de fichiers temporaires, de doublons ou de vieux documents inutiles.
* **Traitement en Masse :** Vous devez appliquer la même opération (renommer, copier, compresser) à des centaines de fichiers.
* **Sauvegarde :** Automatiser la copie de fichiers importants vers un disque de sauvegarde ou un service cloud.
* **Préparation de Données :** Avant d'analyser des données, il faut souvent les consolider depuis plusieurs fichiers.

L'automatisation transforme ces corvées manuelles en opérations fluides et sans effort.

### Concepts Clés

* **Chemin d'Accès (Path) :** L'adresse unique d'un fichier ou d'un dossier sur le système de fichiers (ex: `C:\Users\JohnDoe\Documents\rapport.xlsx` ou `/home/johndoe/reports/report.csv`).
    * **Absolu :** Chemin complet depuis la racine du système.
    * **Relatif :** Chemin par rapport au répertoire courant où le script est exécuté.
* **Répertoire Courant (Current Working Directory - CWD) :** Le dossier depuis lequel votre script Python est exécuté.
* **Opérations de Fichiers :** Création, lecture, écriture, suppression, renommage, copie, déplacement.
* **Opérations de Dossiers :** Création, suppression, liste des contenus.

### Outils Python pour la Manipulation de Fichiers et Dossiers

Python est doté de modules intégrés extrêmement puissants pour gérer le système de fichiers.

#### 1. `os` Module

Le module `os` (Operating System) fournit une interface pour interagir avec le système d'exploitation sous-jacent. Il est parfait pour des opérations basiques sur les chemins, les fichiers et les dossiers.

* **Chemins :** `os.path.join()`, `os.path.exists()`, `os.path.isfile()`, `os.path.isdir()`, `os.path.basename()`, `os.path.dirname()`.
* **Fichiers :** `os.remove()` (supprimer), `os.rename()` (renommer/déplacer).
* **Dossiers :** `os.mkdir()` (créer un dossier), `os.makedirs()` (créer des dossiers récursivement), `os.rmdir()` (supprimer un dossier vide), `os.listdir()` (lister le contenu d'un dossier).

#### 2. `shutil` Module

Le module `shutil` (shell utilities) offre des opérations de plus haut niveau pour la manipulation de fichiers et de collections de fichiers. Il est souvent plus pratique que `os` pour la copie ou le déplacement de dossiers entiers.

* **Copie :** `shutil.copy()` (fichier), `shutil.copytree()` (dossier et son contenu).
* **Déplacement :** `shutil.move()` (fichier ou dossier).
* **Suppression :** `shutil.rmtree()` (supprimer un dossier non vide et son contenu - **À utiliser avec PRUDENCE !**).

#### 3. `pathlib` Module (Recommandé pour la Modernité)

Le module `pathlib` fournit une approche orientée objet pour manipuler les chemins de fichiers, rendant le code plus lisible et moins sujet aux erreurs que les fonctions basées sur des chaînes de caractères du module `os`.

* **Création de chemins :** `Path('mon_dossier') / 'mon_fichier.txt'`
* **Vérification :** `path.exists()`, `path.is_file()`, `path.is_dir()`.
* **Opérations :** `path.mkdir()`, `path.rmdir()`, `path.unlink()` (supprimer un fichier), `path.rename()`.

### Exemples Pratiques d'Automatisation

Créez un dossier `gestion_fichiers_demo/` pour tester les exemples.

```python
# gestion_fichiers_demo/fichier_demo.py
import os
import shutil
from pathlib import Path
import time

# --- Configuration des chemins ---
BASE_DIR = Path("demo_files")
SOURCE_DIR = BASE_DIR / "inbox"
ARCHIVE_DIR = BASE_DIR / "archive"
TEMP_DIR = BASE_DIR / "temp"

# --- 1. Préparation de l'environnement de démo ---
print("--- Préparation de l'environnement de démo ---")
# Créer les dossiers nécessaires si n'existent pas
for d in [SOURCE_DIR, ARCHIVE_DIR, TEMP_DIR]:
    d.mkdir(parents=True, exist_ok=True)
    print(f"Dossier créé ou existant : {d}")

# Créer des fichiers de démo
(SOURCE_DIR / "rapport_ventes_Q1_2024.xlsx").touch()
(SOURCE_DIR / "client_feedback_mars.txt").touch()
(SOURCE_DIR / "old_log_file.log").touch()
(SOURCE_DIR / "image_produit_1.jpg").touch()
print("Fichiers de démo créés dans 'inbox'.")
time.sleep(1) # Pause pour la visibilité

# --- 2. Lister le contenu d'un dossier ---
print("\n--- Listing du contenu de 'inbox' ---")
for item in SOURCE_DIR.iterdir(): # Utilisation de pathlib.Path.iterdir()
    print(f" - {item.name} (est un dossier: {item.is_dir()})")
time.sleep(1)

# --- 3. Déplacer et Renommer un fichier ---
print("\n--- Déplacement et Renommage ---")
old_file_path = SOURCE_DIR / "rapport_ventes_Q1_2024.xlsx"
new_file_path = ARCHIVE_DIR / "ventes_2024_Q1_final.xlsx"

if old_file_path.exists():
    old_file_path.rename(new_file_path) # Déplace et renomme en une seule opération avec pathlib
    print(f"Déplacé et renommé '{old_file_path.name}' vers '{new_file_path}'")
else:
    print(f"Le fichier {old_file_path.name} n'existe pas pour le déplacement.")
time.sleep(1)

# --- 4. Copier un fichier ---
print("\n--- Copie de fichier ---")
source_feedback = SOURCE_DIR / "client_feedback_mars.txt"
destination_temp = TEMP_DIR / "feedback_copy.txt"

if source_feedback.exists():
    shutil.copy(source_feedback, destination_temp) # shutil.copy pour copier un fichier
    print(f"Copié '{source_feedback.name}' vers '{destination_temp}'")
else:
    print(f"Le fichier {source_feedback.name} n'existe pas pour la copie.")
time.sleep(1)

# --- 5. Supprimer un fichier ---
print("\n--- Suppression de fichier ---")
file_to_delete = SOURCE_DIR / "old_log_file.log"

if file_to_delete.exists():
    file_to_delete.unlink() # Supprime un fichier avec pathlib
    print(f"Supprimé : {file_to_delete.name}")
else:
    print(f"Le fichier {file_to_delete.name} n'existe pas pour la suppression.")
time.sleep(1)

# --- 6. Automatisation : Tri de fichiers par extension ---
print("\n--- Automatisation : Tri de fichiers par extension ---")
# Créer quelques fichiers supplémentaires pour le tri
(SOURCE_DIR / "document_A.pdf").touch()
(SOURCE_DIR / "image_2.png").touch()
(SOURCE_DIR / "notes.txt").touch()

# Créer des sous-dossiers pour le tri
for ext_dir_name in ["pdf_files", "image_files", "text_files"]:
    (SOURCE_DIR / ext_dir_name).mkdir(exist_ok=True)

print("Organisation des fichiers dans 'inbox' par type...")
for item in SOURCE_DIR.iterdir():
    if item.is_file(): # Assurez-vous que c'est un fichier
        extension = item.suffix.lower() # .txt, .pdf, .jpg
        if extension == ".pdf":
            item.rename(SOURCE_DIR / "pdf_files" / item.name)
            print(f"  Déplacé {item.name} vers pdf_files/")
        elif extension in [".jpg", ".png", ".gif"]:
            item.rename(SOURCE_DIR / "image_files" / item.name)
            print(f"  Déplacé {item.name} vers image_files/")
        elif extension == ".txt":
            item.rename(SOURCE_DIR / "text_files" / item.name)
            print(f"  Déplacé {item.name} vers text_files/")
        # else:
        #     print(f"  Fichier {item.name} non trié (extension inconnue).")
time.sleep(1)

# --- 7. Nettoyage de l'environnement de démo ---
print("\n--- Nettoyage de l'environnement de démo ---")
shutil.rmtree(BASE_DIR) # Supprime le dossier parent et tout son contenu !
print(f"Dossier '{BASE_DIR}' et son contenu supprimés.")
```

**Pour exécuter cet exemple dans JupyterLab :**

1.  Créez un nouveau notebook dans votre dossier `notebooks/`.
2.  Nommez-le `01_Fichiers_et_Dossiers.ipynb`.
3.  Copiez le code ci-dessus dans une cellule et exécutez-la.
4.  Observez les messages dans la console et explorez votre système de fichiers pour voir les actions en temps réel.

### Conseils et Bonnes Pratiques

* **Chemins Absolus vs Relatifs :** Pour des scripts qui doivent être robustes, privilégiez les chemins absolus ou construisez des chemins relatifs à partir du répertoire du script (`Path(__file__).parent`).
* **Vérification d'Existence :** Toujours vérifier si un fichier ou un dossier existe (`.exists()`, `.is_file()`, `.is_dir()`) avant d'essayer de l'opérer, pour éviter des erreurs.
* **Gestion des Erreurs :** Utilisez des blocs `try-except` pour gérer les `FileNotFoundError`, `PermissionError`, etc.
* **Prudence avec la Suppression :** Les opérations de suppression (`os.remove()`, `shutil.rmtree()`, `path.unlink()`) sont irréversibles. Soyez extrêmement prudent et testez toujours dans un environnement sûr.
* **Utilisez `pathlib` :** C'est l'approche moderne et recommandée en Python pour la manipulation de chemins.

### Tableau Récapitulatif : Opérations Fichiers & Dossiers

| Opération        | Module `os`               | Module `shutil`          | Module `pathlib` (Recommandé) |
| :--------------- | :------------------------ | :----------------------- | :---------------------------- |
| **Créer dossier** | `os.mkdir()`, `os.makedirs()` | N/A                      | `Path.mkdir()`                |
| **Lister contenu** | `os.listdir()`            | N/A                      | `Path.iterdir()`              |
| **Supprimer fichier** | `os.remove()`             | N/A                      | `Path.unlink()`               |
| **Supprimer dossier (vide)** | `os.rmdir()`              | N/A                      | `Path.rmdir()`                |
| **Supprimer dossier (non vide)** | N/A                       | `shutil.rmtree()`        | N/A                           |
| **Renommer/Déplacer** | `os.rename()`             | `shutil.move()`          | `Path.rename()`               |
| **Copier fichier** | N/A                       | `shutil.copy()`, `shutil.copy2()` | `Path.copy_file()` (Python 3.8+) |
| **Copier dossier** | N/A                       | `shutil.copytree()`      | N/A                           |
| **Vérifier existence** | `os.path.exists()`        | N/A                      | `Path.exists()`               |
| **Est un fichier?** | `os.path.isfile()`        | N/A                      | `Path.is_file()`              |
| **Est un dossier?** | `os.path.isdir()`         | N/A                      | `Path.is_dir()`               |
| **Construire chemin** | `os.path.join()`          | N/A                      | `/` (opérateur de division)    |