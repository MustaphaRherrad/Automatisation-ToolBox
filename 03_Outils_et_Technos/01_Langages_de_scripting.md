# Les langages de scripting pour automatiser

Les langages de scripting permettent d’automatiser des actions répétitives, des tâches simples ou des processus complexes, en écrivant quelques lignes de code. En voici les principaux.

## 1. Python : le plus polyvalent

### ✅ Avantages
- Facile à lire et à apprendre.
- Immense écosystème de bibliothèques.
- Très adapté à l'automatisation : fichiers, emails, web scraping, APIs…

### 🔍 Exemples d’usages
- Renommer des fichiers avec `os`.
- Automatiser l’envoi d’e-mails avec `smtplib`.
- Interagir avec des APIs via `requests`.
- Manipuler des fichiers Excel ou CSV avec `pandas`.

## 2. Bash / Shell : le scripting système

### ✅ Avantages
- Parfait pour les environnements Linux/macOS.
- Idéal pour les scripts de maintenance, backup, ou gestion de fichiers.

### ⚠️ Limites
- Moins lisible pour les débutants.
- Moins puissant pour les traitements de données complexes.

### 🔍 Exemple
```bash
#!/bin/bash
for file in *.txt; do
  mv "$file" "${file%.txt}.bak"
done
````

## 3. PowerShell : pour les environnements Windows

* Scripting moderne intégré à Windows.
* Permet de manipuler le système, les fichiers, les registres, Active Directory, etc.
* Syntaxe puissante, mais un peu plus verbeuse.

### 🔍 Exemple

```powershell
Get-ChildItem *.log | Rename-Item -NewName {$_.Name -replace '.log','.bak'}
```

## 4. JavaScript (Node.js) : pour le web et l’automatisation côté serveur

* Idéal si tu viens du développement web.
* Peut être utilisé avec des frameworks comme Puppeteer (automatiser des actions dans un navigateur), ou `node-cron` pour planifier des tâches.

## 5. Autres langages utiles

* **R** : automatisation orientée données/statistiques.
* **Ruby**, **Perl** : anciens mais toujours utilisés dans l’administration système ou le traitement de texte.

---

## En résumé

| Langage    | Idéal pour                            | Niveau requis     |
| ---------- | ------------------------------------- | ----------------- |
| Python     | Automatisations générales, APIs, data | Débutant à avancé |
| Bash       | Scripts système Unix                  | Intermédiaire     |
| PowerShell | Scripts système Windows               | Intermédiaire     |
| JavaScript | Web, automatisation navigateur        | Intermédiaire     |
| R          | Automatisation de traitements stats   | Intermédiaire     |

Tu peux commencer avec **Python**, puis explorer les autres selon ton environnement ou ton projet.
