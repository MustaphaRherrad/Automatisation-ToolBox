# Envoyer des notifications à la fin d’un processus

Une automatisation réussie sait **prévenir** l’utilisateur lorsqu’une tâche est terminée ou qu'une erreur est survenue.

## 1. Notifications locales

Sur Windows ou macOS, on peut envoyer une alerte système :

### Windows :
```python
from plyer import notification

notification.notify(
    title="Tâche terminée",
    message="L'automatisation a bien été exécutée.",
    timeout=5
)
````

### macOS :

```bash
osascript -e 'display notification "Tâche terminée" with title "Script Python"'
```

## 2. Email automatique

```python
import smtplib
from email.mime.text import MIMEText

msg = MIMEText("Le traitement est terminé.")
msg['Subject'] = "Notification automatique"
msg['From'] = "robot@exemple.com"
msg['To'] = "utilisateur@exemple.com"

with smtplib.SMTP('smtp.exemple.com', 587) as server:
    server.starttls()
    server.login("robot@exemple.com", "MOT_DE_PASSE")
    server.send_message(msg)
```

## 3. Notifications via Telegram

Créer un bot avec [@BotFather](https://t.me/BotFather), récupérer le `token` et l’envoyer à un `chat_id`.

```python
import requests

url = f"https://api.telegram.org/bot{TOKEN}/sendMessage"
data = {"chat_id": CHAT_ID, "text": "Traitement terminé avec succès."}
requests.post(url, data=data)
```

## 4. Webhooks

Certains outils (Discord, Slack, Teams) permettent de recevoir des messages via **webhooks**.

```python
import requests

webhook_url = "https://hooks.slack.com/services/XXX/YYY/ZZZ"
data = {"text": "⚙️ Script terminé."}
requests.post(webhook_url, json=data)
```

---

## À retenir

| Type de notification | Usage                  |
| -------------------- | ---------------------- |
| Locale               | Alertes rapides        |
| Email                | Suivi par lot, erreurs |
| Telegram / Discord   | Intégration mobile     |
| Webhooks             | Automatisations pro    |

Une bonne automatisation **parle** à son utilisateur.