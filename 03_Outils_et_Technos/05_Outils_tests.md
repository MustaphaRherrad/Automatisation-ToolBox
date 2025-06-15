# 05 Outils tests
Dans le monde de l'automatisation, tester n'est pas seulement une bonne pratique, c'est une nécessité. Automatiser des tâches, c'est confier des processus critiques à des machines. Sans tests rigoureux, vos automatisations pourraient échouer silencieusement, produire des résultats incorrects, ou même causer des dommages. Les outils de test sont conçus pour vérifier que vos automatisations fonctionnent comme prévu, de manière fiable et constante.

### Pourquoi Tester Vos Automatisations ?

* **Fiabilité :** S'assurer que les automatisations produisent les résultats escomptés à chaque exécution.
* **Détection d'erreurs :** Identifier rapidement les bugs ou les comportements inattendus avant qu'ils ne causent des problèmes majeurs.
* **Robustesse :** Vérifier que les automatisations gèrent correctement les cas limites, les erreurs d'entrée ou les changements inattendus dans l'environnement (par exemple, un site web qui change sa structure).
* **Maintenabilité :** Faciliter les modifications et les mises à jour en s'assurant que les nouvelles fonctionnalités n'introduisent pas de régressions.
* **Confiance :** Gagner la confiance dans vos systèmes automatisés.

### Types de Tests Pertinents pour l'Automatisation

Bien qu'il existe de nombreux types de tests, certains sont particulièrement importants pour l'automatisation des tâches :

* **Tests Unitaires :** Vérifient les plus petites unités de code (fonctions, méthodes) de manière isolée.
* **Tests d'Intégration :** Testent l'interaction entre différentes parties de votre automatisation ou avec des systèmes externes (APIs, bases de données, Ollama).
* **Tests Fonctionnels / de bout en bout :** Simulent le comportement de l'utilisateur final et vérifient que l'automatisation atteint son objectif global (par exemple, un workflow de scraping-résumé-CSV produit un CSV correct).
* **Tests de Régression :** S'assurent que les nouvelles modifications n'ont pas introduit de nouveaux bugs dans les fonctionnalités existantes.

### Outils de Test Populaires pour l'Automatisation

Voici quelques outils clés que vous pouvez utiliser pour tester vos automatisations.

#### 1. Pytest (pour Python)

**Pytest** est un framework de test très populaire et puissant pour Python, idéal pour les tests unitaires et d'intégration de vos scripts d'automatisation.

* **Points forts :**
    * **Facilité d'utilisation :** Très simple à démarrer, avec une syntaxe claire.
    * **Plugins riches :** Extensible via de nombreux plugins pour des fonctionnalités avancées (couverture de code, tests paramétrés, etc.).
    * **Détection automatique :** Découvre automatiquement les tests dans votre projet.
    * **Rapports détaillés :** Fournit des rapports clairs sur les succès et les échecs des tests.
* **Quand l'utiliser :** Pour tester les fonctions individuelles de vos scripts (par exemple, la fonction de scraping qui extrait les liens, ou la fonction de résumé qui appelle Ollama).
* **Exemple de cas d'usage :**
    * Vérifier qu'une fonction `get_article_links` renvoie des liens correctement formatés.
    * Tester qu'une fonction `summarize_text_with_ollama` renvoie une chaîne non vide et plausible pour une entrée donnée.
    * Simuler des réponses d'API ou de site web (avec des "mocks") pour s'assurer que votre script gère différents scénarios.

#### 2. Selenium / Playwright (pour l'Automatisation Web et le Scraping)

Quand vos automatisations interagissent directement avec des navigateurs web (pour le scraping dynamique, la soumission de formulaires), **Selenium** et **Playwright** sont des outils de choix.

* **Points forts :**
    * **Contrôle du navigateur :** Permettent de piloter un navigateur réel (ou headless) pour simuler des interactions utilisateur.
    * **Tests de bout en bout :** Idéal pour tester des workflows d'automatisation qui impliquent la navigation et l'interaction sur des sites web.
    * **Multi-navigateurs :** Supportent différents navigateurs (Chrome, Firefox, Safari).
    * **Playwright :** Est plus récent et souvent considéré comme plus rapide et plus fiable que Selenium pour le scraping et les tests modernes.
* **Quand l'utiliser :**
    * Pour tester si votre script de scraping peut naviguer vers une page, cliquer sur un bouton "charger plus", et extraire les données correctement.
    * Vérifier qu'un workflow d'automatisation de remplissage de formulaire web soumet les données sans erreur.
* **Exemple de cas d'usage :** Automatiser la navigation sur un site d'actualités, cliquer sur un article, et vérifier que le texte de l'article est bien présent sur la page.

#### 3. Requests-mock / Responses (pour les Tests d'API)

Lorsque vos automatisations interagissent avec des APIs (comme l'API locale d'Ollama ou d'autres services web), il est crucial de pouvoir simuler ces interactions sans faire de vraies requêtes. Les bibliothèques de *mocking* comme **`requests-mock`** ou **`responses`** (pour la bibliothèque `requests` de Python) sont parfaites pour cela.

* **Points forts :**
    * **Isolation :** Permettent de tester votre code sans dépendre de la disponibilité ou du comportement réel des services externes.
    * **Contrôle total :** Vous définissez exactement quelles réponses les requêtes recevront, y compris les erreurs.
    * **Rapidité :** Les tests s'exécutent beaucoup plus vite car ils ne font pas de vraies requêtes réseau.
* **Quand l'utiliser :** Pour tester que votre fonction `summarize_text_with_ollama` gère correctement les réponses réussies d'Ollama, mais aussi les erreurs réseau ou les réponses malformées.
* **Exemple de cas d'usage :** Écrire un test qui simule une réponse réussie d'Ollama avec un résumé attendu, puis un test qui simule une erreur 500 de l'API Ollama pour s'assurer que votre code gère l'exception.

### Intégration des Tests dans Votre Projet

* **Structure des tests :** Créez un dossier `tests/` à la racine de votre projet. À l'intérieur, organisez vos fichiers de test de manière logique (par exemple, `tests/test_scraping.py`, `tests/test_ollama_integration.py`).
* **Exécution :** Lancez vos tests depuis le terminal avec `pytest` dans le répertoire de votre projet.
* **Intégration continue (CI) :** Pour les projets plus avancés, les tests peuvent être intégrés dans un pipeline d'intégration continue (par exemple, GitHub Actions, GitLab CI) pour s'exécuter automatiquement à chaque modification de code.

Investir du temps dans l'écriture de tests pour vos automatisations vous fera gagner énormément de temps à long terme en évitant les problèmes et en garantissant la qualité de vos processus.
