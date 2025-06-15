La gestion des données est un pilier de toute automatisation qui traite des informations de manière structurée et persistante. Les bases de données, qu'elles soient relationnelles (SQL) ou non-relationnelles (NoSQL), offrent des moyens puissants de stocker, d'organiser et de récupérer des données. Ce tutoriel vous donnera les bases pour interagir avec ces systèmes depuis Python.

### Pourquoi Utiliser des Bases de Données pour l'Automatisation ?

* **Persistance des Données :** Les données sont stockées de manière fiable et ne sont pas perdues après l'exécution du script.
* **Structuration :** Les données sont organisées de manière logique, ce qui facilite les requêtes et les analyses.
* **Scalabilité :** Les bases de données peuvent gérer de grandes quantités de données et un grand nombre d'accès simultanés.
* **Intégrité :** Les bases de données garantissent la cohérence et la validité des données (contraintes, transactions).
* **Accès Concurrentiel :** Plusieurs processus ou utilisateurs peuvent accéder aux données en même temps sans conflit.

### Concepts Clés

#### Bases de Données Relationnelles (SQL)

* **Tables :** Les données sont organisées en tables, composées de lignes (enregistrements) et de colonnes (champs).
* **Schéma :** La structure de la base de données (tables, colonnes, types de données, relations).
* **SQL (Structured Query Language) :** Le langage standard pour interagir avec les bases de données relationnelles.
    * **SELECT :** Récupérer des données.
    * **INSERT :** Ajouter de nouvelles données.
    * **UPDATE :** Modifier des données existantes.
    * **DELETE :** Supprimer des données.
* **Relations :** Les tables peuvent être liées entre elles (par exemple, une table "clients" et une table "commandes").
* **Transactions :** Un ensemble d'opérations traitées comme une seule unité logique (tout réussit ou tout échoue).

#### Bases de Données Non-Relationnelles (NoSQL)

* **Différents Modèles :**
    * **Document :** Les données sont stockées sous forme de documents (JSON-like). (ex: MongoDB).
    * **Clé-Valeur :** Les données sont stockées sous forme de paires clé-valeur. (ex: Redis).
    * **Graphe :** Conçues pour les relations complexes (ex: Neo4j).
* **Flexibilité du Schéma :** Pas de schéma fixe, ce qui permet de stocker des données variées et évolutives.
* **Scalabilité Horizontale :** Faciles à distribuer sur plusieurs serveurs.

### Outils Python pour les Bases de Données

#### Bases de Données Relationnelles (SQL)

* **`sqlite3` (SQLite) :** Bibliothèque intégrée pour travailler avec des bases de données SQLite (idéales pour les petits projets ou le stockage local).
* **`psycopg2` (PostgreSQL) :** Adaptateur pour PostgreSQL (très robuste et performant).
* **`mysql-connector-python` (MySQL) :** Connecteur pour MySQL.
* **`SQLAlchemy` :** Un ORM (Object-Relational Mapper) puissant. Permet d'interagir avec les bases de données en utilisant des objets Python plutôt que des requêtes SQL directes.

#### Bases de Données Non-Relationnelles (NoSQL)

* **`pymongo` (MongoDB) :** Bibliothèque officielle pour MongoDB.

### Exemples Pratiques d'Automatisation

#### Exemple 1 : Stocker des Données Web Scrapées dans une Base de Données SQLite

```python
# databases_demo.py (partie 1)
import sqlite3
import requests
from bs4 import BeautifulSoup

def scrape_and_store_prices(url, product_selector, db_file):
    """Scrape les prix d'un site web et les stocke dans une base de données SQLite."""
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        soup = BeautifulSoup(response.text, 'html.parser')
        
        price_element = soup.select_one(product_selector)
        if price_element:
            price = price_element.get_text(strip=True)
            print(f"Prix trouvé sur {url} : {price}")
        else:
            print(f"Prix non trouvé sur {url} avec le sélecteur '{product_selector}'.")
            return

        conn = sqlite3.connect(db_file)
        cursor = conn.cursor()

        # Créer la table si elle n'existe pas
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS product_prices (
                url TEXT PRIMARY KEY,
                price TEXT,
                timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
            )
        """)

        # Insérer ou mettre à jour le prix
        cursor.execute("""
            INSERT OR REPLACE INTO product_prices (url, price)
            VALUES (?, ?)
        """, (url, price))

        conn.commit()
        print(f"Prix stocké dans la base de données '{db_file}'.")
        conn.close()

    except requests.exceptions.RequestException as e:
        print(f"Erreur de requête HTTP: {e}")
    except sqlite3.Error as e:
        print(f"Erreur de base de données: {e}")

# Utilisation (adaptez l'URL et le sélecteur)
# URL_PRODUIT = "https://example.com/produit/super-gadget"
# SELECTEUR_PRIX = "span.product-price"
# DB_FILE = "product_prices.db"
# scrape_and_store_prices(URL_PRODUIT, SELECTEUR_PRIX, DB_FILE)
```

#### Exemple 2 : Stocker des Logs dans MongoDB

```python
# databases_demo.py (partie 2)
from pymongo import MongoClient
import datetime

def store_log_in_mongodb(log_message, db_name, collection_name, mongo_uri="mongodb://localhost:27017/"):
    """Stocke un message de log dans MongoDB."""
    try:
        client = MongoClient(mongo_uri)
        db = client[db_name]
        logs = db[collection_name]

        log_entry = {
            "message": log_message,
            "timestamp": datetime.datetime.now()
        }

        result = logs.insert_one(log_entry)
        print(f"Log stocké dans MongoDB (ID: {result.inserted_id}).")
        client.close()

    except Exception as e:
        print(f"Erreur lors de l'interaction avec MongoDB: {e}")

# Utilisation (assurez-vous que MongoDB est lancé)
# store_log_in_mongodb("Le processus de sauvegarde a démarré.", "my_app_logs", "app_events")
# store_log_in_mongodb("Erreur critique: Impossible de se connecter au serveur.", "my_app_logs", "app_events")
```

**Pour exécuter ces exemples dans JupyterLab :**

1.  Créez un nouveau notebook `06_Gestion_des_Données_Databases.ipynb`.
2.  Collez le code des fonctions dans des cellules séparées.
3.  Exécutez les cellules.
    * Pour l'exemple SQLite, un fichier `product_prices.db` sera créé dans le même répertoire.
    * Pour l'exemple MongoDB, assurez-vous que MongoDB est installé et lancé sur votre machine.

### Conseils et Bonnes Pratiques

* **Sécurité :** Ne jamais coder en dur les informations de connexion à la base de données (mots de passe, URLs) dans votre code. Utilisez des variables d'environnement.
* **Gestion des Erreurs :** Toujours gérer les exceptions liées à la base de données (connexion, requêtes).
* **Paramétrisation des Requêtes :** Pour éviter les injections SQL, utilisez toujours des requêtes paramétrées (avec `?` ou `%s`) plutôt que de construire des chaînes SQL dynamiquement.
* **Transactions :** Pour les opérations critiques, utilisez des transactions pour garantir la cohérence des données.
* **ORM (SQLAlchemy) :** Pour les applications complexes, un ORM peut simplifier la manipulation des données et rendre votre code plus portable entre différents systèmes de bases de données.
* **NoSQL vs SQL :** Choisissez le type de base de données en fonction de vos besoins. SQL pour des données structurées et des relations complexes, NoSQL pour des données plus flexibles et une meilleure scalabilité.

### Tableau Récapitulatif : Outils de Gestion de Données

| Type de Base de Données | Outils Python Principaux | Cas d'Usage Typique                                  | Avantages                                      | Inconvénients                                         |
| :---------------------- | :----------------------- | :--------------------------------------------------- | :--------------------------------------------- | :-------------------------------------------------- |
| **SQL (Relationnelles)** | `sqlite3`, `psycopg2`, `mysql-connector-python`, `SQLAlchemy` | Stockage de données structurées, relations, transactions | Intégrité des données, requêtes puissantes, standard | Schéma rigide, moins de flexibilité pour les données non structurées |
| **NoSQL (Non-Relationnelles)** | `pymongo`                | Stockage de données semi-structurées, scalabilité, flexibilité | Schéma flexible, facile à faire évoluer, scalable | Moins de garanties sur l'intégrité des données, moins standardisé |
