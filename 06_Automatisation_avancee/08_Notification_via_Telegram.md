# 09 Notification via Telegram

Dans le monde de l'automatisation, la rapidité et la fiabilité des notifications sont primordiales. Au-delà des e-mails, les applications de messagerie instantanée comme Telegram offrent un canal direct et efficace pour recevoir des alertes, des rapports et même interagir avec vos automatisations. Ce tutoriel vous montrera comment intégrer des notifications Telegram dans vos scripts Python.

### Pourquoi Utiliser Telegram pour les Notifications ?

* **Rapidité :** Les messages sont livrés instantanément.
* **Fiabilité :** Telegram est connu pour sa robustesse.
* **Accessibilité :** Notifications sur smartphone, tablette et desktop.
* **Richesses des Messages :** Supporte le texte formaté, les images, les fichiers, les boutons interactifs.
* **Interactivité :** Possibilité de créer des bots pour répondre à des commandes et déclencher des actions.
* **Gratuit et Ouvert :** L'API Bot est gratuite et bien documentée.

### Concepts Clés de l'API Telegram Bot

* **Bot :** Un compte spécial Telegram qui peut envoyer et recevoir des messages de manière programmatique.
* **BotFather :** Un bot Telegram officiel qui vous aide à créer et gérer vos propres bots. C'est le point de départ pour obtenir votre **token de bot**.
* **Token de Bot :** Une chaîne de caractères unique (ex: `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`) qui sert d'identifiant et de clé d'authentification pour votre bot. **Gardez-le secret !**
* **Chat ID :** L'identifiant unique d'une conversation (chat) avec votre bot. Il peut être l'ID d'un utilisateur, d'un groupe ou d'un canal. Vous en aurez besoin pour savoir où envoyer vos messages.
* **Webhooks vs. Long Polling :**
    * **Webhooks :** Telegram envoie des notifications à une URL de votre serveur chaque fois qu'un événement se produit (message reçu). Meilleur pour les applications en production.
    * **Long Polling :** Votre script interroge régulièrement l'API Telegram pour de nouveaux messages. Plus simple pour les scripts ponctuels ou les petits projets.

```mermaid
graph TD
    A[Votre Script Python] --> B(Appel API Telegram Bot);
    B -- Token + Chat ID + Message --> C[Serveurs Telegram];
    C -- Message Push --> D[Client Telegram (Votre Téléphone/PC)];
```
*Figure 14 : Flux d'envoi de notification Telegram*

### Étapes pour Configurer Votre Bot Telegram

1.  **Créer un Bot avec BotFather :**
    * Ouvrez Telegram et recherchez `@BotFather`.
    * Lancez une conversation avec lui et tapez `/newbot`.
    * Suivez les instructions pour choisir un nom et un nom d'utilisateur pour votre bot.
    * BotFather vous donnera le **token d'API** de votre bot. **Copiez-le et gardez-le précieusement.**

2.  **Obtenir Votre Chat ID :**
    * Lancez une conversation avec votre nouveau bot (cliquez sur son nom d'utilisateur dans le message de BotFather).
    * Envoyez n'importe quel message à votre bot (ex: "Salut").
    * Ouvrez votre navigateur et allez à l'URL suivante (remplacez `YOUR_BOT_TOKEN` par votre token réel) :
        `https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates`
    * Cherchez la section `"chat"` dans la réponse JSON. L'`"id"` à l'intérieur de cette section est votre **Chat ID**. S'il y a un message sous `message.from.id` c'est votre `chat_id`

### Outils Python pour Telegram

* **`requests` :** Pour interagir directement avec l'API REST de Telegram (très simple pour l'envoi de messages).
* **`python-telegram-bot` :** Une bibliothèque plus complète et de plus haut niveau pour construire des bots interactifs.

### Exemple Pratique : Envoyer une Notification Simple

Nous utiliserons la méthode la plus simple avec `requests` pour envoyer un message.

**Pré-requis :**
1.  Votre **token de bot Telegram**.
2.  Votre **Chat ID** Telegram.
3.  Installez `requests` : `pip install requests`
4.  Mettez votre token et Chat ID dans votre fichier `.env` (très important pour la sécurité) :
    ```dotenv
    # .env
    TELEGRAM_BOT_TOKEN="YOUR_BOT_TOKEN_HERE"
    TELEGRAM_CHAT_ID="YOUR_CHAT_ID_HERE"
    ```

**Fichier `send_telegram_notification.py` :**

```python
# telegram_notifications/send_telegram_notification.py
import requests
import os
from dotenv import load_dotenv

load_dotenv() # Charge les variables d'environnement depuis .env

def send_telegram_message(message):
    """
    Envoie un message texte à un chat Telegram via l'API Bot.
    """
    bot_token = os.getenv("TELEGRAM_BOT_TOKEN")
    chat_id = os.getenv("TELEGRAM_CHAT_ID")

    if not bot_token or not chat_id:
        print("Erreur: Les variables d'environnement TELEGRAM_BOT_TOKEN ou TELEGRAM_CHAT_ID ne sont pas configurées.")
        return False

    telegram_api_url = f"https://api.telegram.org/bot{bot_token}/sendMessage"
    
    payload = {
        "chat_id": chat_id,
        "text": message,
        "parse_mode": "HTML" # Permet d'utiliser du HTML pour le formatage (ex: <b>texte gras</b>)
    }

    try:
        response = requests.post(telegram_api_url, json=payload, timeout=10)
        response.raise_for_status() # Lève une erreur pour les codes d'état 4xx ou 5xx
        print("Message Telegram envoyé avec succès.")
        return True
    except requests.exceptions.RequestException as e:
        print(f"Erreur lors de l'envoi du message Telegram: {e}")
        if response.status_code == 401:
            print("Vérifiez que votre TELEGRAM_BOT_TOKEN est correct.")
        elif response.status_code == 404:
            print("Vérifiez que votre TELEGRAM_CHAT_ID est correct et que le bot a commencé une conversation avec ce chat.")
        return False
    except Exception as e:
        print(f"Une erreur inattendue est survenue: {e}")
        return False

# --- Utilisation ---
if __name__ == "__main__":
    print("--- Test de l'envoi de notification Telegram ---")
    
    # Message de test simple
    send_telegram_message("Salut depuis mon script d'automatisation Python ! 👋")

    # Message plus complexe avec formatage HTML
    html_message = (
        "<b>Rapport Quotidien :</b>\n\n"
        "🟢 Tous les services sont <i>opérationnels</i>.\n"
        "📊 Nombre de transactions aujourd'hui : <b>1234</b>.\n"
        "🔗 <a href='https://example.com/rapport'>Voir le rapport complet</a>"
    )
    send_telegram_message(html_message)

    # Message d'erreur
    # send_telegram_message("🚨 ALERTE CRITIQUE : Le serveur de production est en panne !!!")
```

**Pour exécuter cet exemple dans JupyterLab :**

1.  Créez un dossier `telegram_notifications/`.
2.  Créez votre fichier `.env` à l'intérieur avec vos identifiants.
3.  Créez le fichier `send_telegram_notification.py` à l'intérieur.
4.  Dans votre notebook, vous pouvez importer la fonction et l'appeler :
    ```python
    # Dans votre notebook Jupyter
    import sys
    sys.path.append('./telegram_notifications') # Pour que Python trouve votre module
    from send_telegram_notification import send_telegram_message

    send_telegram_message("Message de test depuis Jupyter !")
    ```
5.  Exécutez la cellule et vérifiez votre client Telegram.

### Conseils et Bonnes Pratiques

* **Sécurité du Token :** Ne jamais exposer votre token de bot. Utilisez toujours des variables d'environnement (`.env`).
* **Chat ID Correct :** Assurez-vous d'avoir le bon `chat_id`. Le bot doit avoir été contacté au moins une fois par le destinataire (ou être membre d'un groupe/canal et avoir les permissions).
* **Fréquence des Messages :** Évitez d'envoyer trop de messages trop rapidement, au risque d'être limité par Telegram.
* **Contenu du Message :** Adaptez le message au contexte : soyez concis pour les alertes, plus détaillé pour les rapports. Utilisez le formatage HTML ou Markdown pour la lisibilité.
* **Bots Interactifs :** Pour des scénarios plus avancés (répondre à des commandes, boutons), explorez la bibliothèque `python-telegram-bot`.
* **Gestion des Erreurs :** Anticipez les problèmes (réseau, token invalide, chat_id incorrect) et loguez les erreurs.

### Tableau Récapitulatif : Notifications Telegram

| Caractéristique | Description                                       | Outils Python |
| :-------------- | :------------------------------------------------ | :------------ |
| **Création Bot** | Via `@BotFather` sur Telegram                     | N/A           |
| **Obtention Token** | Donné par `@BotFather`                            | N/A           |
| **Obtention Chat ID** | Via `getUpdates` ou un bot pour ID (`@userinfobot`) | N/A           |
| **Envoi Messages** | Requêtes HTTP `POST` à l'API Telegram (`sendMessage`) | `requests`    |
| **Sécurité** | Utilisation de variables d'environnement (`.env`) | `dotenv`      |
| **Formatage Message** | HTML ou Markdown (`parse_mode` parameter)         | N/A           |
| **Interaction Avancée** | Réception de messages, boutons, commandes         | `python-telegram-bot` |

---
