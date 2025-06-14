# Outils de gestion de workflows

Un **workflow** est une suite d’étapes automatisées. Les outils de gestion de workflows permettent de concevoir, déclencher, et superviser ces étapes.

## 1. Zapier, Make : automatisation visuelle

- Connectent des services sans coder.
- Workflow = une série de déclencheurs + actions.
- Adaptés à la bureautique, au marketing, aux tâches simples.

### Exemple : Zapier
- Trigger : un nouvel email arrive dans Gmail.
- Action : le contenu est ajouté dans Notion.
- Suivi via tableau de bord.

## 2. Airflow : pour les pipelines de données

- Créé par Airbnb, utilisé pour orchestrer des tâches complexes.
- Workflow = DAG (Directed Acyclic Graph).
- Chaque étape peut être un script Python, un appel API, une requête SQL.

### Avantages :
- Solide pour les traitements réguliers (batch).
- Monitoring, relances en cas d’échec.

## 3. Node-RED : visualisation pour IoT et APIs

- Basé sur JavaScript.
- Interface en glisser-déposer.
- Utile pour connecter des capteurs, des APIs, des scripts locaux.

## 4. Apache NiFi

- Très utilisé pour les flux de données temps réel.
- Interface visuelle.
- Gestion des erreurs et du débit inclus.

## 5. Outils orientés entreprise

- **Power Automate (Microsoft)** : workflows sur Office, SharePoint, Teams.
- **UiPath** : RPA (automatisation des gestes humains).
- **n8n** : open-source, workflows complexes avec hébergement local possible.

---

## En résumé

| Outil          | Public cible       | Type de tâches                          |
|----------------|--------------------|-----------------------------------------|
| Zapier / Make  | Débutants / bureautique | E-mails, CRM, stockage cloud…       |
| Airflow        | Data engineers     | Pipelines de données, ETL              |
| Node-RED       | Dév / IoT          | Capteurs, APIs, événements              |
| NiFi           | Entreprise / Data  | Flux massifs en temps réel              |
| Power Automate | Utilisateurs Office| Tâches bureautiques / Microsoft 365     |
| n8n            | Tech + open source | Connecteurs + logique personnalisée     |

Si tu veux automatiser une tâche multi-étapes, ces outils t’éviteront de réécrire des scripts manuels à chaque fois.
