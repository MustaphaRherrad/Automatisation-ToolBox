# Automatiser des appels à une API

Les API permettent à tes scripts d’interagir avec des services web. Les automatiser ouvre la porte à des intégrations puissantes : récupération de données, envoi de commandes, déclenchement d’actions…

## 1. Qu’est-ce qu’une API ?

Une API (Application Programming Interface) est une interface accessible par un programme pour échanger des données avec un autre système.

- Une API REST (la plus fréquente) fonctionne avec des requêtes HTTP (`GET`, `POST`, `PUT`, `DELETE`).
- Les données sont souvent échangées au format JSON.

## 2. Requête simple avec Python

```python
import requests

url = "https://api.exemple.com/utilisateurs"
response = requests.get(url)
data = response.json()
print(data)
````

## 3. Authentification

Certaines API demandent un token ou une clé API.

```python
headers = {"Authorization": "Bearer MON_TOKEN"}
response = requests.get(url, headers=headers)
```

## 4. Exemple concret : API météo

```python
url = "https://api.weatherapi.com/v1/current.json"
params = {"key": "CLE_API", "q": "Paris"}
r = requests.get(url, params=params)
print(r.json())
```

## 5. Automatiser les appels

* Enregistrer la réponse dans un fichier :

```python
with open("meteo.json", "w") as f:
    json.dump(r.json(), f)
```

* Lancer le script chaque jour via `cron` ou `Task Scheduler`.

## 6. Bonnes pratiques

| Pratique             | Pourquoi ?                   |
| -------------------- | ---------------------------- |
| Gérer les erreurs    | API parfois indisponible     |
| Respecter les quotas | Pour ne pas se faire bloquer |
| Utiliser le cache    | Éviter les appels inutiles   |

---

**Automatiser une API, c’est transformer un service en source de données vivante.**
