# 01 Scripts reutilisables

Au fil de ce guide, vous avez découvert et implémenté de nombreux fragments de code et des scripts complets. Cette section regroupe des scripts génériques et polyvalents, conçus pour être facilement réutilisés et adaptés à diverses situations d'automatisation. Considérez-les comme des "modèles" que vous pouvez copier, coller et modifier pour vos propres besoins.

### Pourquoi des Scripts Réutilisables ?

  * **Gain de temps :** Plus besoin de réinventer la roue pour des tâches courantes.
  * **Standardisation :** Utiliser des bases éprouvées pour des tâches récurrentes.
  * **Point de départ :** Servent de tremplin pour des automatisations plus complexes.
  * **Bonnes pratiques :** Illustrent des façons robustes et propres de coder.

### 1\. Gestion des Fichiers et Dossiers

Ce script offre des fonctions pour manipuler le système de fichiers, essentielles dans presque toutes les automatisations.

```python
# scripts_reutilisables/file_manager.py
import os
import shutil
import datetime
from pathlib import Path

def create_directory(path: str | Path):
    """Crée un dossier si celui-ci n'existe pas déjà."""
    path = Path(path)
    if not path.exists():
        path.mkdir(parents=True, exist_ok=True)
        print(f"Dossier créé : {path}")
    else:
        print(f"Le dossier existe déjà : {path}")

def move_file(source_path: str | Path, destination_path: str | Path):
    """Déplace un fichier d'un emplacement à un autre."""
    source_path = Path(source_path)
    destination_path = Path(destination_path)
    
    if not source_path.exists():
        print(f"Erreur : Le fichier source n'existe pas : {source_path}")
        return

    try:
        shutil.move(str(source_path), str(destination_path))
        print(f"Fichier déplacé de '{source_path}' vers '{destination_path}'")
    except Exception as e:
        print(f"Erreur lors du déplacement du fichier {source_path} : {e}")

def copy_file(source_path: str | Path, destination_path: str | Path):
    """Copie un fichier d'un emplacement à un autre."""
    source_path = Path(source_path)
    destination_path = Path(destination_path)
    
    if not source_path.exists():
        print(f"Erreur : Le fichier source n'existe pas : {source_path}")
        return

    try:
        shutil.copy2(str(source_path), str(destination_path)) # copy2 conserve les métadonnées
        print(f"Fichier copié de '{source_path}' vers '{destination_path}'")
    except Exception as e:
        print(f"Erreur lors de la copie du fichier {source_path} : {e}")

def delete_file(path: str | Path):
    """Supprime un fichier."""
    path = Path(path)
    if path.exists() and path.is_file():
        try:
            os.remove(str(path))
            print(f"Fichier supprimé : {path}")
        except Exception as e:
            print(f"Erreur lors de la suppression du fichier {path} : {e}")
    else:
        print(f"Le fichier n'existe pas ou n'est pas un fichier : {path}")

def delete_directory(path: str | Path, ignore_errors: bool = False):
    """Supprime un dossier et son contenu."""
    path = Path(path)
    if path.exists() and path.is_dir():
        try:
            shutil.rmtree(str(path), ignore_errors=ignore_errors)
            print(f"Dossier supprimé : {path}")
        except Exception as e:
            print(f"Erreur lors de la suppression du dossier {path} : {e}")
    else:
        print(f"Le dossier n'existe pas ou n'est pas un dossier : {path}")

def list_directory_contents(path: str | Path, include_files: bool = True, include_dirs: bool = True):
    """Liste le contenu d'un dossier."""
    path = Path(path)
    if not path.is_dir():
        print(f"Erreur : Le chemin n'est pas un dossier : {path}")
        return []

    contents = []
    for item in path.iterdir():
        if include_files and item.is_file():
            contents.append(item.name)
        elif include_dirs and item.is_dir():
            contents.append(item.name)
    return contents

if __name__ == "__main__":
    # Exemples d'utilisation
    base_dir = Path("./temp_automation_files")
    create_directory(base_dir)
    create_directory(base_dir / "sub_folder")

    # Création de fichiers factices
    (base_dir / "test_file.txt").write_text("Ceci est un fichier test.")
    (base_dir / "another_file.log").write_text("Log entry.")

    print("\nContenu initial :")
    print(list_directory_contents(base_dir, include_dirs=True, include_files=True))

    move_file(base_dir / "test_file.txt", base_dir / "sub_folder" / "moved_file.txt")
    copy_file(base_dir / "another_file.log", base_dir / "copied_log.log")

    print("\nContenu après déplacement et copie :")
    print(list_directory_contents(base_dir, include_dirs=True, include_files=True))
    print(list_directory_contents(base_dir / "sub_folder", include_dirs=True, include_files=True))

    delete_file(base_dir / "copied_log.log")
    delete_directory(base_dir / "sub_folder")

    print("\nContenu final :")
    print(list_directory_contents(base_dir, include_dirs=True, include_files=True))

    delete_directory(base_dir)
```

### 2\. Gestion des E-mails

Ces fonctions simplifient l'envoi d'e-mails, une tâche très courante en automatisation pour les notifications ou les rapports.

```python
# scripts_reutilisables/email_sender.py
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders
import os
from dotenv import load_dotenv

load_dotenv() # Assurez-vous que votre fichier .env est configuré

def send_email(subject: str, body: str, to_email: str, 
               attachments: list = None,
               is_html: bool = False):
    """
    Envoie un e-mail avec ou sans pièces jointes.
    Les identifiants SMTP sont lus depuis les variables d'environnement.
    """
    sender_email = os.getenv("EMAIL_SENDER")
    sender_password = os.getenv("EMAIL_PASSWORD")
    smtp_server = os.getenv("SMTP_SERVER")
    smtp_port = int(os.getenv("SMTP_PORT", 587))

    if not all([sender_email, sender_password, smtp_server, to_email]):
        print("Erreur: Les informations d'email sont incomplètes dans le .env ou l'appel de fonction.")
        return False

    msg = MIMEMultipart()
    msg['From'] = sender_email
    msg['To'] = to_email
    msg['Subject'] = subject

    # Définir le contenu du corps de l'email
    if is_html:
        msg.attach(MIMEText(body, 'html', 'utf-8'))
    else:
        msg.attach(MIMEText(body, 'plain', 'utf-8'))

    # Ajouter les pièces jointes
    if attachments:
        for attachment_path in attachments:
            if not os.path.exists(attachment_path):
                print(f"Avertissement: La pièce jointe '{attachment_path}' n'existe pas et sera ignorée.")
                continue
            
            try:
                with open(attachment_path, "rb") as attachment:
                    part = MIMEBase("application", "octet-stream")
                    part.set_payload(attachment.read())
                encoders.encode_base64(part)
                part.add_header(
                    "Content-Disposition",
                    f"attachment; filename= {os.path.basename(attachment_path)}",
                )
                msg.attach(part)
            except Exception as e:
                print(f"Erreur lors de l'ajout de la pièce jointe {attachment_path}: {e}")
                continue

    try:
        with smtplib.SMTP(smtp_server, smtp_port) as server:
            server.starttls()  # Démarrer le chiffrement TLS
            server.login(sender_email, sender_password)
            server.send_message(msg)
        print(f"Email '{subject}' envoyé avec succès à {to_email}.")
        return True
    except Exception as e:
        print(f"Échec de l'envoi de l'e-mail : {e}")
        return False

if __name__ == "__main__":
    # Assurez-vous d'avoir un fichier .env avec :
    # EMAIL_SENDER=votre_email@example.com
    # EMAIL_PASSWORD=votre_mot_de_passe_app_ou_mdp_normal (attention à la sécurité)
    # SMTP_SERVER=smtp.example.com (ex: smtp.gmail.com)
    # SMTP_PORT=587

    # Exemple d'utilisation simple
    recipient = os.getenv("EMAIL_RECEIVER", "votre.email.test@example.com") 
    send_email(
        subject="Test Automatisation Python",
        body="Ceci est un e-mail de test envoyé par votre script Python d'automatisation.",
        to_email=recipient
    )

    # Exemple avec pièce jointe
    # Créez un fichier factice pour le test
    with open("test_attachment.txt", "w") as f:
        f.write("Ceci est un contenu de pièce jointe test.")
    
    send_email(
        subject="Test avec Pièce Jointe",
        body="Ceci est un e-mail avec une pièce jointe.",
        to_email=recipient,
        attachments=["test_attachment.txt"]
    )
    os.remove("test_attachment.txt") # Nettoyer le fichier test
```

### 3\. Requêtes HTTP Généralistes

Un script utilitaire pour effectuer des requêtes web, très utile pour interagir avec des APIs ou scraper des données.

```python
# scripts_reutilisables/web_requests.py
import requests
import json

def fetch_data(url: str, params: dict = None, headers: dict = None, timeout: int = 10):
    """
    Effectue une requête GET et retourne le contenu JSON si disponible, sinon le texte.
    Gère les erreurs réseau et les timeouts.
    """
    print(f"Récupération de données depuis : {url}")
    try:
        response = requests.get(url, params=params, headers=headers, timeout=timeout)
        response.raise_for_status() # Lève une exception pour les codes d'état HTTP d'erreur (4xx ou 5xx)
        
        try:
            return response.json()
        except json.JSONDecodeError:
            print(f"La réponse n'est pas un JSON valide. Retourne le texte brut.")
            return response.text
            
    except requests.exceptions.HTTPError as errh:
        print(f"Erreur HTTP: {errh}")
    except requests.exceptions.ConnectionError as errc:
        print(f"Erreur de connexion: {errc}")
    except requests.exceptions.Timeout as errt:
        print(f"Timeout: {errt}")
    except requests.exceptions.RequestException as err:
        print(f"Erreur inattendue: {err}")
    return None

def post_data(url: str, data: dict = None, headers: dict = None, timeout: int = 10):
    """
    Effectue une requête POST avec des données JSON.
    Retourne le contenu JSON de la réponse si disponible.
    """
    print(f"Envoi de données vers : {url}")
    try:
        response = requests.post(url, json=data, headers=headers, timeout=timeout)
        response.raise_for_status()
        
        try:
            return response.json()
        except json.JSONDecodeError:
            print(f"La réponse n'est pas un JSON valide. Retourne le texte brut.")
            return response.text

    except requests.exceptions.HTTPError as errh:
        print(f"Erreur HTTP: {errh}")
    except requests.exceptions.ConnectionError as errc:
        print(f"Erreur de connexion: {errc}")
    except requests.exceptions.Timeout as errt:
        print(f"Timeout: {errt}")
    except requests.exceptions.RequestException as err:
        print(f"Erreur inattendue: {err}")
    return None

if __name__ == "__main__":
    # Exemple d'utilisation de fetch_data
    print("--- Test de récupération de données (GET) ---")
    data = fetch_data("https://jsonplaceholder.typicode.com/todos/1")
    if data:
        print("Données récupérées :", data)

    print("\n--- Test d'envoi de données (POST) ---")
    new_post = {
        "title": "foo",
        "body": "bar",
        "userId": 1
    }
    response_post = post_data("https://jsonplaceholder.typicode.com/posts", data=new_post)
    if response_post:
        print("Réponse POST :", response_post)

    print("\n--- Test d'erreur HTTP ---")
    error_data = fetch_data("https://jsonplaceholder.typicode.com/nonexistent") # Ceci devrait provoquer une erreur 404
    if error_data is None:
        print("Erreur gérée comme prévu pour l'URL inexistante.")
```

### 4\. Envoi de Notifications Telegram

Un moyen rapide et efficace de recevoir des alertes ou des mises à jour de vos scripts.

```python
# scripts_reutilisables/telegram_notifier.py
import requests
import os
from dotenv import load_dotenv

load_dotenv() # Assurez-vous que votre fichier .env est configuré

def send_telegram_message(message: str, chat_id: str = None, bot_token: str = None):
    """
    Envoie un message à un chat Telegram spécifique via un bot.
    Les identifiants sont lus depuis les variables d'environnement si non fournis.
    """
    # Utiliser les arguments ou les variables d'environnement
    final_chat_id = chat_id if chat_id else os.getenv("TELEGRAM_CHAT_ID")
    final_bot_token = bot_token if bot_token else os.getenv("TELEGRAM_BOT_TOKEN")

    if not all([final_chat_id, final_bot_token]):
        print("Erreur: TELEGRAM_CHAT_ID ou TELEGRAM_BOT_TOKEN non configuré dans .env ou non fourni.")
        print("Veuillez consulter la section 'Notifications Telegram' pour la configuration.")
        return False

    url = f"https://api.telegram.org/bot{final_bot_token}/sendMessage"
    payload = {
        "chat_id": final_chat_id,
        "text": message,
        "parse_mode": "Markdown" # Permet d'utiliser du Markdown dans le message
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status() # Lève une exception pour les codes d'état HTTP d'erreur
        if response.json().get('ok'):
            print(f"Message Telegram envoyé avec succès au chat {final_chat_id}.")
            return True
        else:
            print(f"Échec de l'envoi du message Telegram: {response.json().get('description', 'Inconnu')}")
            return False
    except requests.exceptions.RequestException as e:
        print(f"Erreur réseau ou HTTP lors de l'envoi Telegram: {e}")
        return False

if __name__ == "__main__":
    # Pour utiliser ce script, vous devez obtenir un TELEGRAM_BOT_TOKEN et un TELEGRAM_CHAT_ID.
    # 1. Parlez à BotFather sur Telegram pour créer un nouveau bot et obtenir votre TELEGRAM_BOT_TOKEN.
    # 2. Démarrez une conversation avec votre nouveau bot, puis allez sur https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates
    #    (remplacez <YOUR_BOT_TOKEN> par le vrai token) pour trouver votre TELEGRAM_CHAT_ID.
    # 3. Ajoutez TELEGRAM_BOT_TOKEN et TELEGRAM_CHAT_ID à votre fichier .env.

    # Exemple d'utilisation
    send_telegram_message("🔔 *Alerte* : Votre script d'automatisation a terminé son exécution !")
    send_telegram_message("Rapport généré avec succès. Vérifiez votre boîte mail.")

    # Exemple de message d'erreur
    # send_telegram_message("❌ *Erreur critique* : Le scraping a échoué. Veuillez vérifier les logs.", 
    #                       chat_id="VOTRE_ID_CHAT_TEST", bot_token="VOTRE_TOKEN_BOT_TEST") # Overrides .env for testing
```
