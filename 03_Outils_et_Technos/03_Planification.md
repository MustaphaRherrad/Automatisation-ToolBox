# Planification et déclenchement des automatisations

Une automatisation ne sert à rien si elle ne s’exécute pas au bon moment. Il existe plusieurs manières de planifier ou déclencher des tâches automatiquement.

## 1. Planifier une exécution dans le temps

### 🕓 Cron (Linux/macOS)
- Format simple mais puissant.
- Exemples :
  - Tous les jours à 8h : `0 8 * * *`
  - Tous les lundis : `0 9 * * 1`

### 🗓️ Windows Task Scheduler
- Interface graphique pour planifier l’exécution d’un script, programme ou tâche Windows.
- Peut s’appuyer sur des événements système.

### 📅 Alternatives
- `at` (Unix) : pour une exécution unique.
- `launchd` (macOS) : système plus avancé que `cron`.

## 2. Déclenchement événementiel

### 🔔 Webhooks
- Un service envoie une requête à ton script dès qu’un événement se produit.
- Très utilisé dans Zapier, GitHub, Stripe, Notion, etc.
- Ex : lorsqu’un formulaire est rempli → envoyer un message Slack.

### 🔁 Trigger dans outils no-code
- Exemple : “quand un fichier est ajouté à Google Drive”.
- La tâche s’exécute à chaque changement observé.

## 3. Boucles d’écoute ou polling

- Vérifie toutes les X minutes si une condition est remplie.
- Exemple : un script qui consulte une API toutes les 10 minutes.
- ⚠️ Consomme plus de ressources que les webhooks.

## 4. Programmation dans le code

- En Python : avec `schedule`, `APScheduler`, ou un `while True` avec `sleep`.
- En JavaScript : avec `setInterval`, `cron`, ou des librairies Node.

---

## En résumé

| Méthode                 | Utilité                           | Exemples                          |
|-------------------------|------------------------------------|-----------------------------------|
| Cron / Scheduler        | Exécution récurrente dans le temps| Backup, nettoyage, script régulier|
| Webhooks                | Déclenchement en temps réel       | Nouvel utilisateur, nouveau mail  |
| Polling                 | Surveillance régulière            | Suivi d’un stock, météo, API      |
| Librairies de planning  | Intégration dans le code          | Bots, systèmes embarqués          |

Choisis ta méthode selon le **contexte** :
- Action ponctuelle ou régulière ?
- Déclenchement par événement ou horloge ?
- Script local ou hébergé ?

L’essentiel est d’avoir le bon _timing_ pour ne rien faire trop tôt, ni trop tard.
