# Outils utiles pour automatiser

L’automatisation peut se faire avec ou sans code. Voici une sélection d’outils couramment utilisés, classés selon les profils utilisateurs et les usages.

## 1. Outils sans code (no-code)

Idéaux pour débuter sans compétences techniques. Ils permettent de créer des automatisations visuelles avec des blocs logiques.

### 🔹 Zapier
- Connecte des centaines d’applications web.
- Exemple : quand un nouvel e-mail arrive → enregistrer l’attachement sur Google Drive → notifier sur Slack.
- Facile à utiliser, version gratuite limitée.

### 🔹 Make (ex-Integromat)
- Plus visuel et puissant que Zapier.
- Permet des scénarios complexes, des branches conditionnelles, des boucles.
- Interface plus technique.

### 🔹 Microsoft Power Automate
- Intégré à Microsoft 365.
- Parfait pour automatiser des tâches sur SharePoint, Outlook, Teams, Excel…
- Interface drag-and-drop, version gratuite incluse dans certaines licences Office.

### 🔹 IFTTT
- Simplicité maximale.
- Ciblé grand public : automatiser des actions entre objets connectés, services simples (Gmail, météo, réseaux sociaux…).

## 2. Outils avec code (script, CLI, API)

Pour ceux qui souhaitent aller plus loin avec du Python, du Shell ou des scripts.

### 🔸 Python
- Langage idéal pour automatiser avec précision.
- Bibliothèques populaires :
  - `os`, `shutil` : manipuler des fichiers.
  - `pandas` : traitement de données.
  - `smtplib`, `email` : envoi d’e-mails.
  - `requests` : interaction avec des APIs.

### 🔸 Bash / Shell / PowerShell
- Pour automatiser en ligne de commande :
  - Renommer des fichiers.
  - Créer des backups.
  - Planifier des tâches via `cron` (Linux/macOS) ou `Task Scheduler` (Windows).

### 🔸 APIs REST
- Permettent d’automatiser l’interaction avec d’autres services (Slack, Notion, GitHub, Google Drive…).
- Utilisables depuis presque tous les langages.

## 3. Outils pour planifier des tâches

- **Cron (Linux)** : tâches répétitives selon un calendrier.
- **Task Scheduler (Windows)** : équivalent pour les utilisateurs Windows.
- **n8n** : alternative open-source à Zapier/Make.

## 4. Outils de RPA (Robotic Process Automation)

- **UIPath**, **Automation Anywhere**, **Blue Prism**
- Simulent l’action humaine (clics, saisies clavier).
- Adaptés aux logiciels internes sans API.

---

## En résumé

| Objectif                       | Outil(s) recommandé(s)                   |
|-------------------------------|------------------------------------------|
| Débuter sans coder            | Zapier, Make, Power Automate, IFTTT      |
| Automatiser avec code         | Python, Bash, APIs                       |
| Automatiser dans Microsoft 365| Power Automate                           |
| Simuler un humain             | UIPath, RPA tools                        |
| Planifier une exécution       | Cron, Task Scheduler                     |

Le choix des outils dépend :
- Du niveau technique,
- De la fréquence et de la complexité des tâches,
- Des applications à connecter.

Tu n’as pas besoin de tous les connaître. Commence par un ou deux, puis élargis ton arsenal au fil des besoins.


