# 03 Glossaire
Ce glossaire regroupe les termes techniques clés utilisés tout au long de ce guide. Il est conçu pour vous aider à vous familiariser avec le jargon de l'automatisation et de la programmation Python.

-----

**API (Application Programming Interface) :** Un ensemble de règles et de définitions qui permet à différentes applications logicielles de communiquer entre elles. Dans l'automatisation, nous utilisons souvent des APIs pour interagir avec des services web.

**Automatisation :** Le processus de faire exécuter une tâche ou une série de tâches par une machine ou un logiciel, sans intervention humaine.

**Bibliothèque (Library) :** Un ensemble de modules et de fonctions pré-écrits que les développeurs peuvent utiliser pour étendre les capacités de Python sans avoir à coder chaque fonctionnalité de zéro. (Ex: `requests`, `pandas`).

**CI/CD (Continuous Integration / Continuous Deployment) :** Des pratiques de développement logiciel qui visent à automatiser et rationaliser le processus de livraison de logiciels, de l'intégration du code au déploiement en production.

**CLI (Command Line Interface) :** Une interface utilisateur textuelle pour interagir avec un programme ou un système d'exploitation en tapant des commandes.

**Conteneurisation :** Le processus d'encapsulation d'une application et de toutes ses dépendances (code, bibliothèques, configurations) dans un package léger et portable appelé **conteneur** (ex: Docker).

**Cron Job :** Un utilitaire sur les systèmes d'exploitation de type Unix (Linux, macOS) qui permet de planifier l'exécution automatique de commandes ou de scripts à des heures ou des intervalles spécifiés.

**Dépendances :** D'autres modules ou bibliothèques dont votre script a besoin pour fonctionner. Elles sont généralement listées dans un fichier `requirements.txt`.

**Déploiement :** Le processus de mise en place d'une application ou d'un script dans un environnement d'exécution (par exemple, un serveur de production) où il peut être utilisé par les utilisateurs ou fonctionner de manière autonome.

**Docker :** Une plateforme logicielle qui utilise la conteneurisation pour faciliter la création, le déploiement et l'exécution d'applications.

**`.env` :** Un fichier texte simple utilisé pour stocker les variables d'environnement spécifiques à un projet, souvent pour des secrets ou des configurations, qui ne devraient pas être commises dans le contrôle de version.

**Environnement Virtuel (Virtual Environment) :** Un répertoire auto-contenu qui contient une installation de Python et des bibliothèques spécifiques à un projet, isolant ainsi les dépendances de ce projet des autres projets ou de l'installation Python du système.

**ETL (Extract, Transform, Load) :** Un processus en trois étapes pour déplacer des données d'une source vers un entrepôt de données. **Extraction** (récupération des données), **Transformation** (nettoyage, enrichissement), **Chargement** (écriture dans la destination).

**Framework :** Un cadre de travail structuré qui fournit une base pour le développement d'applications, offrant des fonctionnalités et des conventions pour accélérer le processus (ex: Django, Flask pour le web).

**IDE (Integrated Development Environment) :** Un environnement de développement intégré qui fournit des outils complets pour le développement logiciel, incluant un éditeur de code, un débogueur, un compilateur, etc. (ex: PyCharm, VS Code).

**Infrastructure as Code (IaC) :** La gestion de l'infrastructure informatique (réseaux, serveurs, bases de données) en utilisant des fichiers de configuration lisibles par machine plutôt que des configurations manuelles ou interactives.

**Logs (Journaux) :** Des enregistrements textuels ou structurés des événements qui se produisent pendant l'exécution d'un programme, utilisés pour le débogage, le diagnostic et la surveillance.

**Low-Code :** Des plateformes de développement qui nécessitent un minimum de codage manuel, souvent grâce à des interfaces visuelles et des composants pré-faits, mais qui permettent toujours des extensions via du code si nécessaire.

**Markdown :** Un langage de balisage léger qui permet de formater du texte en utilisant une syntaxe simple et facile à lire (ex: ce guide est écrit en Markdown).

**Métriques :** Des données numériques mesurables qui décrivent les performances ou le comportement d'un système ou d'une application (ex: temps d'exécution, utilisation CPU).

**Monitoring (Surveillance) :** Le processus de collecte et d'analyse de données (métriques, logs) sur l'état et les performances d'un système ou d'une application pour détecter les problèmes et assurer son bon fonctionnement.

**No-Code :** Des plateformes qui permettent de créer des applications ou d'automatiser des processus sans écrire une seule ligne de code, en utilisant principalement des interfaces glisser-déposer et des configurations.

**Open Source :** Logiciel dont le code source est librement accessible, modifiable et redistribuable par quiconque.

**Orchestration :** La coordination et la gestion automatisée de plusieurs systèmes ou services informatiques pour qu'ils fonctionnent ensemble de manière cohérente (ex: orchestration de conteneurs avec Kubernetes).

**`pip` :** L'installateur de paquets par défaut pour Python, utilisé pour installer et gérer les bibliothèques tierces.

**Pipelines :** Une série d'étapes automatisées qui sont exécutées séquentiellement pour accomplir une tâche plus grande (ex: un pipeline de données ETL, un pipeline CI/CD).

**Prod (Production) :** L'environnement où le logiciel est réellement utilisé par les utilisateurs finaux ou où l'automatisation s'exécute de manière opérationnelle.

**Refactoring :** Le processus de restructuration du code existant sans modifier son comportement externe, dans le but d'améliorer sa lisibilité, sa maintenabilité ou sa performance.

**Régression (Bug de) :** Un bug qui réapparaît ou une fonctionnalité qui cesse de fonctionner après une modification du code, alors qu'elle fonctionnait correctement auparavant.

**`requests` :** Une bibliothèque Python populaire pour effectuer des requêtes HTTP (GET, POST, etc.) et interagir avec des services web.

**`requirements.txt` :** Un fichier texte qui liste toutes les dépendances Python nécessaires à un projet, permettant de recréer l'environnement du projet.

**SDK (Software Development Kit) :** Un ensemble d'outils de développement logiciel qui permet la création d'applications pour une plateforme spécifique (ex: AWS SDK pour interagir avec les services Amazon).

**Scraping (Web Scraping) :** Le processus d'extraction automatisée de données à partir de sites web, souvent en analysant leur structure HTML.

**Secrets :** Des informations sensibles (mots de passe, clés API, jetons d'authentification) qui doivent être protégées et ne jamais être codées en dur dans les scripts.

**SMTP (Simple Mail Transfer Protocol) :** Le protocole standard utilisé pour l'envoi d'e-mails.

**Tests Unitaires :** Des tests qui vérifient de petites unités de code (fonctions, méthodes) de manière isolée pour s'assurer qu'elles fonctionnent comme prévu.

**Versionnement (Version Control) :** La pratique de gérer et de suivre les changements apportés à un code source ou à des documents au fil du temps (ex: Git).

**Workflow :** Une séquence d'activités, de tâches ou d'étapes interconnectées qui, lorsqu'elles sont exécutées, accomplissent un processus ou atteignent un objectif spécifique.
