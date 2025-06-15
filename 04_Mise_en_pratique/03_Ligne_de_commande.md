# 03 Ligne de commande
La ligne de commande, souvent perçue comme austère, est en réalité l'un des outils les plus puissants et flexibles pour l'automatisation. Elle vous permet d'interagir directement avec le système d'exploitation, d'exécuter des programmes, de manipuler des fichiers, et d'orchestrer des processus. En Python, vous pouvez lancer et contrôler des commandes shell, ce qui ouvre un monde de possibilités pour intégrer vos scripts avec des outils système ou d'autres applications.

### Pourquoi Maîtriser la Ligne de Commande en Automatisation ?

* **Contrôle Système :** Exécuter des commandes système (créer des dossiers, lister des processus, gérer des services).
* **Intégration d'Outils :** Lancer d'autres programmes ou scripts écrits dans des langages différents (Bash, PowerShell, Java, etc.).
* **Pipelines de Données :** Chaîner des outils en ligne de commande (comme `grep`, `awk`, `sed`) pour traiter des données.
* **Déploiement et Administration :** Automatiser des tâches de déploiement de logiciels, de gestion de serveurs, ou de configurations réseau.
* **Flexibilité :** Accéder à des fonctionnalités qui n'ont pas forcément d'équivalent direct en bibliothèques Python.

### Concepts Clés

* **Shell :** L'interpréteur de commandes (ex: Bash sous Linux/macOS, PowerShell/CMD sous Windows).
* **Commande Externe :** Un programme exécutable (ex: `ls`, `dir`, `git`, `python`).
* **Arguments :** Des informations passées à une commande pour modifier son comportement (ex: `ls -l`, `-l` est un argument).
* **Code de Retour (Exit Code) :** Une valeur numérique renvoyée par un programme à la fin de son exécution. `0` indique généralement un succès, toute autre valeur un échec. C'est crucial pour la gestion d'erreurs en automatisation.
* **Entrée/Sortie Standard (Stdin/Stdout/Stderr) :**
    * **Stdout (Standard Output) :** Là où un programme écrit sa sortie normale (messages, résultats).
    * **Stderr (Standard Error) :** Là où un programme écrit ses messages d'erreur.
    * **Stdin (Standard Input) :** L'entrée que le programme lit (rarement utilisé pour les scripts d'automatisation simples).

### Outils Python pour la Ligne de Commande

Python offre plusieurs façons d'interagir avec la ligne de commande, avec des niveaux de contrôle différents.

#### 1. `os.system()` (Simplicité, mais Moins de Contrôle)

C'est la méthode la plus simple pour exécuter une commande, mais elle offre peu de contrôle sur le processus.

* **Utilisation :**
    ```python
    import os
    # Exécute la commande et affiche sa sortie directement dans la console
    exit_code = os.system("ls -l") # Sous Linux/macOS
    # exit_code = os.system("dir") # Sous Windows
    print(f"Code de retour: {exit_code}")
    ```
* **Points forts :** Extrêmement simple à utiliser.
* **Limitations :** Difficile de capturer la sortie (stdout/stderr) ou de gérer les erreurs de manière granulaire. Le script Python attend la fin de l'exécution de la commande.

#### 2. `subprocess.run()` (Recommandé, Contrôle Complet)

Le module `subprocess` est la manière **recommandée** et la plus moderne d'exécuter des commandes externes. Il offre un contrôle complet sur l'exécution, la capture de la sortie, et la gestion des erreurs.

* **Utilisation de base :**
    ```python
    import subprocess

    # Exécution simple, affiche la sortie et lève une erreur si la commande échoue
    try:
        result = subprocess.run(["ls", "-l"], check=True, capture_output=True, text=True) # Sous Linux/macOS
        # result = subprocess.run(["dir"], check=True, capture_output=True, text=True, shell=True) # Sous Windows
        print(f"Sortie standard:\n{result.stdout}")
        print(f"Erreur standard:\n{result.stderr}")
        print(f"Code de retour: {result.returncode}")
    except subprocess.CalledProcessError as e:
        print(f"La commande a échoué avec le code {e.returncode}. Erreur:\n{e.stderr}")
    except FileNotFoundError:
        print("Commande introuvable. Vérifiez votre PATH.")
    ```
    * `check=True` : Lève une `CalledProcessError` si la commande renvoie un code de retour non nul.
    * `capture_output=True` : Capture `stdout` et `stderr`.
    * `text=True` : Décode la sortie en texte (sinon, ce sont des bytes).
    * `shell=True` : Exécute la commande via le shell. Souvent nécessaire sous Windows pour des commandes simples. **Attention :** peut être un risque de sécurité si la commande contient des entrées utilisateur non validées. Préférez `shell=False` avec la commande et ses arguments sous forme de liste.

```mermaid
graph TD
    A[Script Python] --> B{subprocess.run()};
    B -- Exécute --> C[Commande Externe (ex: `git pull`)];
    C -- Stdout/Stderr --> B;
    C -- Code de Retour --> B;
    B -- Capture Sortie --> D[Variable Python (`result.stdout`)];
    B -- Gère Code de Retour --> E[Log Erreur / Continue le Workflow];
```
*Figure 10 : Interaction avec la ligne de commande via `subprocess.run()`*

### Exemples Pratiques d'Automatisation

#### Exemple 1 : Récupérer l'État d'un Dépôt Git

```python
# Ligne_de_commande_demo.py (partie 1)
import subprocess
import os

def get_git_status(repo_path):
    """Récupère le statut d'un dépôt Git."""
    if not os.path.exists(os.path.join(repo_path, ".git")):
        print(f"'{repo_path}' n'est pas un dépôt Git valide.")
        return None

    try:
        # Assurez-vous que Git est installé et dans votre PATH
        result = subprocess.run(
            ["git", "-C", repo_path, "status", "--short"], # -C pour changer de répertoire
            check=True,
            capture_output=True,
            text=True
        )
        status_output = result.stdout.strip()
        if status_output:
            print(f"Modifications dans '{repo_path}':\n{status_output}")
        else:
            print(f"'{repo_path}' est à jour (pas de modifications en attente).")
        return status_output
    except subprocess.CalledProcessError as e:
        print(f"Erreur lors de l'exécution de 'git status' : {e.stderr}")
        return None
    except FileNotFoundError:
        print("Commande 'git' non trouvée. Assurez-vous que Git est installé et dans votre PATH.")
        return None

# Pour tester, créez un dossier, puis `git init` dedans
# mkdir my_test_repo
# cd my_test_repo
# git init
# echo "Hello" > test.txt
# python votre_script.py (lancez depuis le dossier parent)
# get_git_status("my_test_repo")
```

#### Exemple 2 : Exécuter un Autre Script Python avec des Arguments

Vous pouvez lancer un autre script Python comme une commande externe.

```python
# Ligne_de_commande_demo.py (partie 2)

# Créez un petit script cible pour cet exemple:
# # target_script.py
# import sys
# print(f"Salut de target_script.py ! Arguments reçus: {sys.argv[1:]}")
# if "erreur" in sys.argv:
#    sys.exit(1) # Simuler une erreur

def run_target_script(args):
    """Exécute un script Python externe avec des arguments."""
    command = ["python", "target_script.py"] + args
    print(f"\nExécution de la commande : {' '.join(command)}")
    try:
        result = subprocess.run(command, check=True, capture_output=True, text=True)
        print(f"Sortie du script:\n{result.stdout}")
        print(f"Code de retour: {result.returncode}")
    except subprocess.CalledProcessError as e:
        print(f"Le script a échoué avec le code {e.returncode}. Erreur:\n{e.stderr}")
    except FileNotFoundError:
        print("Interpréteur 'python' ou 'target_script.py' non trouvé.")

# Utilisation
# run_target_script(["--message", "Bonjour le monde", "--version", "1.0"])
# run_target_script(["--message", "Ceci va causer une erreur", "erreur"])
```

**Pour exécuter ces exemples dans JupyterLab :**

1.  Créez un nouveau notebook `03_Ligne_de_commande.ipynb`.
2.  Créez un dossier `demo_cli/` à côté de votre dossier `notebooks/`.
3.  Dans `demo_cli/`, créez un fichier `target_script.py` avec le contenu indiqué dans l'exemple 2.
4.  Dans `demo_cli/`, créez un dossier `my_test_repo/` et `git init` dedans pour l'exemple 1.
5.  Collez le code des fonctions `get_git_status` et `run_target_script` dans des cellules séparées.
6.  Dans des cellules distinctes, appelez les fonctions avec leurs exemples d'utilisation.

### Conseils et Bonnes Pratiques

* **Sécurité (`shell=True`) :** N'utilisez `shell=True` que si vous êtes absolument sûr que la commande et ses arguments ne proviennent pas d'une source non fiable (entrées utilisateur), car cela peut exposer à des vulnérabilités d'injection de commandes. Préférez toujours une liste d'arguments (`["commande", "arg1", "arg2"]`).
* **Erreurs et Codes de Retour :** Toujours vérifier les codes de retour pour s'assurer que la commande s'est exécutée avec succès.
* **Capturer la Sortie :** Utilisez `capture_output=True` pour lire le `stdout` et `stderr` de la commande et les traiter dans votre script.
* **Logging :** Enregistrez les commandes exécutées, leurs sorties et les erreurs dans vos logs pour faciliter le débogage.
* **Environnement :** Soyez conscient de l'environnement (variables PATH) dans lequel la commande est exécutée.

### Tableau Récapitulatif : Interactions Ligne de Commande

| Fonction Python  | Simplicité | Contrôle sur l'exécution | Capture Sortie | Gestion Erreurs Facile | Utilisation Recommandée |
| :--------------- | :--------- | :----------------------- | :------------- | :--------------------- | :---------------------- |
| `os.system()`    | Haute      | Bas                      | Non            | Non                    | Commandes très simples, pas de besoin de sortie |
| `subprocess.run()` | Moyenne    | Haute (avec `check`, `capture_output`) | Oui            | Oui                    | **Toutes les automatisations sérieuses** |
