# 02 Surveillance et logs
Une fois votre automatisation en production, la question n'est plus seulement de savoir si elle s'exécute, mais comment elle s'exécute. La surveillance (monitoring) et la gestion des logs sont essentielles pour comprendre le comportement de vos scripts, détecter les problèmes rapidement et garantir leur bon fonctionnement continu.

### Pourquoi la Surveillance et les Logs sont Indispensables ?

* **Détection des Pannes :** Identifier immédiatement si une automatisation a échoué ou ne s'est pas exécutée du tout.
* **Diagnostic des Problèmes :** Comprendre la cause racine d'un échec grâce aux informations détaillées des logs.
* **Performance :** Mesurer le temps d'exécution, l'utilisation des ressources, et identifier les goulots d'étranglement.
* **Audit et Conformité :** Avoir une trace des actions effectuées par l'automatisation.
* **Amélioration Continue :** Utiliser les données de surveillance pour optimiser et affiner vos scripts.
* **Visibilité :** Offrir une vue d'ensemble de l'état de santé de toutes vos automatisations.

### Concepts Clés

* **Logs (Journaux) :** Des enregistrements textuels ou structurés des événements qui se produisent pendant l'exécution d'un script (messages d'information, avertissements, erreurs).
* **Niveaux de Log :** Différents niveaux de sévérité pour les messages (DEBUG, INFO, WARNING, ERROR, CRITICAL).
* **Agrégation de Logs :** Centraliser les logs de plusieurs sources en un seul endroit pour faciliter la recherche et l'analyse.
* **Métriques :** Données numériques mesurables sur la performance ou le comportement de votre automatisation (temps d'exécution, nombre de succès/échecs, utilisation CPU/mémoire).
* **Alerting (Alertes) :** Des notifications automatiques déclenchées lorsque certaines conditions prédéfinies sont atteintes (ex: script échoue 3 fois de suite, temps d'exécution dépasse X minutes).
* **Tableaux de Bord (Dashboards) :** Visualisations graphiques des métriques et des statuts des automatisations.

```mermaid
graph TD
    A[Script d'Automatisation] --> B(Génère des Logs);
    B -- Vers --> C[Système de Log (Fichier, Console)];
    C -- Collecte et Envoi --> D[Système d'Agrégation de Logs];
    D -- Alerting Rules --> E[Système d'Alerting];
    E -- Notifications --> F[Équipe Opérationnelle];
    D -- Visualisation --> G[Tableau de Bord];
```
*Figure 18 : Flux de surveillance et de logs d'une automatisation*

### Outils et Méthodes pour la Surveillance et les Logs

#### 1. Logging en Python (`logging` Module)

Le module `logging` de Python est la manière standard et la plus efficace de gérer les logs dans vos scripts.

* **Configuration de Base :**
    ```python
    import logging

    # Configuration simple pour envoyer les logs à la console
    logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

    # Utilisation
    logging.debug("Ceci est un message de debug.")
    logging.info("L'opération a commencé.")
    logging.warning("Un problème mineur a été rencontré.")
    logging.error("Une erreur critique est survenue !")
    logging.critical("Le système est en panne.")
    ```
* **Logging vers un Fichier :**
    ```python
    import logging

    logging.basicConfig(filename='my_automation.log', level=logging.INFO, 
                        format='%(asctime)s - %(levelname)s - %(message)s')
    logging.info("Démarrage du script.")
    ```
* **Meilleures Pratiques :**
    * Utilisez toujours le module `logging`.
    * Choisissez les niveaux de log appropriés pour chaque message.
    * Capturez les exceptions dans les logs avec `logging.exception()`.
    * Incluez des informations contextuelles (ID de transaction, nom d'utilisateur) dans vos messages.

#### 2. Collecte et Agrégation de Logs

Pour les environnements de production, vous ne voulez pas seulement des fichiers de logs locaux, mais un système centralisé.

* **ELK Stack (Elasticsearch, Logstash, Kibana) :** Très populaire pour la centralisation, le traitement et la visualisation des logs.
* **Grafana Loki :** Système d'agrégation de logs léger, souvent utilisé avec Grafana pour la visualisation.
* **Splunk, Datadog, Sumo Logic :** Solutions commerciales complètes pour l'observabilité.
* **Services Cloud de Logs :**
    * **AWS CloudWatch Logs / Google Cloud Logging / Azure Monitor Logs :** Services natifs des fournisseurs cloud pour collecter, stocker et analyser les logs.

#### 3. Métriques et Alerting

* **Prometheus / Grafana :** Combinaison open-source très populaire pour collecter des métriques et créer des tableaux de bord et des alertes.
* **Datadog, New Relic, Dynatrace :** Plateformes d'observabilité commerciales offrant une surveillance complète.
* **Services Cloud de Métriques et Alertes :**
    * **AWS CloudWatch Metrics & Alarms :** Pour surveiller les ressources AWS et déclencher des alertes.
    * **Google Cloud Monitoring / Azure Monitor :** Équivalents chez GCP et Azure.

### Exemple Pratique : Logging Avancé avec Python

Nous allons améliorer notre script de rapport quotidien pour inclure un logging structuré et des exemples de gestion d'erreurs.

**Fichier `my_daily_report_with_logs.py` :**

```python
# monitoring_demo/my_daily_report_with_logs.py
import datetime
import os
import smtplib
from email.mime.text import MIMEText
import logging
from dotenv import load_dotenv

# --- Configuration du logging ---
# Crée un dossier 'logs' si il n'existe pas
log_dir = 'logs'
if not os.path.exists(log_dir):
    os.makedirs(log_dir)

# Nom du fichier de log avec la date du jour
log_file = os.path.join(log_dir, f"report_{datetime.date.today().strftime('%Y%m%d')}.log")

# Configuration du logger principal
logging.basicConfig(
    level=logging.INFO, # Niveau de log minimal affiché (INFO et plus)
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler(log_file), # Envoie les logs dans un fichier
        logging.StreamHandler()        # Envoie aussi les logs à la console
    ]
)

# Obtenir un logger pour ce module
logger = logging.getLogger(__name__)

# Charge les variables d'environnement
load_dotenv() 

def generate_report():
    """Simule la génération d'un rapport quotidien."""
    logger.info("Début de la génération du rapport.")
    report_date = datetime.date.today().strftime("%Y-%m-%d")
    report_content = f"Rapport quotidien généré le {report_date}.\n\n" \
                     f"Données collectées à {datetime.datetime.now().strftime('%H:%M:%S')}.\n" \
                     f"Ceci est un rapport de test avec logging avancé."
    
    # Simuler une condition d'erreur aléatoire (pour démonstration)
    if datetime.datetime.now().second % 2 == 0:
        logger.warning("Condition rare rencontrée. Veuillez vérifier les données sources.")
        # Pour forcer une erreur pour les tests
        # raise ValueError("Erreur de simulation pour le test!")

    logger.info(f"Rapport généré avec succès pour le {report_date}.")
    return report_content

def send_report_email(subject, body, to_email):
    """Envoie le rapport par e-mail."""
    logger.info(f"Tentative d'envoi du rapport par e-mail à {to_email}.")
    sender_email = os.getenv("EMAIL_SENDER")
    sender_password = os.getenv("EMAIL_PASSWORD")
    smtp_server = os.getenv("SMTP_SERVER")
    smtp_port = int(os.getenv("SMTP_PORT", 587))

    if not all([sender_email, sender_password, smtp_server, to_email]):
        logger.error("Informations d'email incomplètes. L'e-mail ne peut pas être envoyé.")
        return False

    msg = MIMEText(body, 'plain', 'utf-8')
    msg['Subject'] = subject
    msg['From'] = sender_email
    msg['To'] = to_email

    try:
        with smtplib.SMTP(smtp_server, smtp_port) as server:
            server.starttls()
            server.login(sender_email, sender_password)
            server.send_message(msg)
        logger.info(f"Rapport '{subject}' envoyé avec succès.")
        return True
    except Exception as e:
        logger.exception(f"Échec de l'envoi du rapport par e-mail: {e}") # logging.exception inclut la stack trace
        return False

if __name__ == "__main__":
    logger.info("Démarrage du script de génération de rapport quotidien.")
    
    try:
        report = generate_report()
        
        receiver_email = os.getenv("EMAIL_RECEIVER") 
        if receiver_email:
            send_report_email(f"Rapport Quotidien - {datetime.date.today().strftime('%Y-%m-%d')}", report, receiver_email)
        else:
            logger.warning("EMAIL_RECEIVER non configuré dans .env. Le rapport ne sera pas envoyé par e-mail.")
        
        logger.info("Script terminé avec succès.")

    except Exception as e:
        logger.critical(f"Le script a rencontré une erreur fatale et s'est arrêté: {e}", exc_info=True)
        # Vous pourriez ajouter ici un mécanisme d'alerte critique (ex: envoyer un SMS)
```

**Pour tester dans JupyterLab :**

1.  Créez un dossier `monitoring_demo/`.
2.  Créez un fichier `.env` avec vos informations email (pour que l'envoi d'email fonctionne).
3.  Créez le fichier `my_daily_report_with_logs.py` à l'intérieur.
4.  Exécutez la cellule suivante :
    ```python
    # Dans une cellule Jupyter
    import sys
    sys.path.append('./monitoring_demo')
    import my_daily_report_with_logs # Exécute le script
    ```
5.  Vérifiez la sortie de la cellule et examinez le fichier de log créé dans le dossier `monitoring_demo/logs/`. Vous devriez voir les messages `INFO` et `WARNING` (si la condition de seconde est remplie). Si vous décommentez la `ValueError` dans `generate_report`, vous verrez un `ERROR` et un `CRITICAL` avec la stack trace.

### Tableau Récapitulatif : Surveillance et Logs

| Aspect             | Objectif                                       | Outils Python                | Outils Externes/Cloud       |
| :----------------- | :--------------------------------------------- | :--------------------------- | :-------------------------- |
| **Génération Logs** | Enregistrer les événements du script          | `logging` module             | N/A                         |
| **Collecte Logs** | Centraliser les logs de multiples sources    | (via agents ou libs spécifiques) | ELK Stack, Grafana Loki, CloudWatch Logs, Datadog |
| **Métriques** | Mesurer la performance et le comportement     | (via libs comme `prometheus_client` ou clients d'APIs) | Prometheus, Grafana, CloudWatch Metrics, Datadog |
| **Alerting** | Notifier les problèmes critiques               | (Via les outils externes)     | Alertmanager, CloudWatch Alarms, Datadog |
| **Tableaux de Bord** | Visualiser l'état et la performance des automatisations | (via les outils externes)     | Grafana, Kibana, Datadog    |
