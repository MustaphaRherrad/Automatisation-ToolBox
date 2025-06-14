# Exemple complet d’automatisation de A à Z

Pour illustrer les principes vus précédemment, voici un exemple simple d’automatisation d’un processus : télécharger un rapport, le traiter, et l’envoyer par e-mail.

## Objectif

Chaque lundi :
1. Télécharger un fichier CSV depuis une URL.
2. Calculer des statistiques.
3. Générer un graphique.
4. Envoyer le résultat par email.

## Étape 1 : Télécharger le fichier

```python
import requests
url = "https://exemple.com/data.csv"
r = requests.get(url)
with open("data.csv", "wb") as f:
    f.write(r.content)
````

## Étape 2 : Analyser les données

```python
import pandas as pd
df = pd.read_csv("data.csv")
resume = df.describe()
```

## Étape 3 : Générer un graphique

```python
import matplotlib.pyplot as plt
df['valeur'].plot(kind='line')
plt.title("Évolution")
plt.savefig("graphique.png")
```

## Étape 4 : Envoyer le mail

```python
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg['Subject'] = 'Rapport hebdo'
msg['From'] = 'monemail@example.com'
msg['To'] = 'destinataire@example.com'
msg.set_content("Bonjour, voici le rapport.")

with open("graphique.png", "rb") as f:
    msg.add_attachment(f.read(), maintype='image', subtype='png', filename='graphique.png')

with smtplib.SMTP_SSL('smtp.example.com', 465) as smtp:
    smtp.login('monemail@example.com', 'motdepasse')
    smtp.send_message(msg)
```

## Étape 5 : Planification

* Sur Linux : ajouter une ligne dans `crontab` :

```bash
0 9 * * 1 python3 script.py
```

---

## Résultat

✅ Le lundi à 9h, un mail est envoyé automatiquement avec le rapport et le graphique attaché.

Ce type de script est modulaire et peut être adapté pour d’autres fichiers, d’autres fréquences, ou d’autres canaux d’envoi.