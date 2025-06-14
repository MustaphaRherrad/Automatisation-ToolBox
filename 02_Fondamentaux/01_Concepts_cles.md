# Concepts clés à connaître

Avant de plonger dans les outils, quelques concepts fondamentaux permettent de mieux comprendre comment fonctionne l’automatisation.

## 1. Tâche

Une tâche est une **unité d’action** : copier un fichier, envoyer un mail, extraire des données, convertir un format…

Elle peut être :
- Simple (déplacer un fichier),
- Composée (télécharger un fichier, l’ouvrir, extraire des données, envoyer un e-mail).

## 2. Workflow

Un workflow est une **séquence de tâches** organisées pour atteindre un objectif.

> _Exemple :_
> 1. Récupérer un fichier CSV
> 2. Le nettoyer
> 3. Générer un graphique
> 4. Envoyer le tout par e-mail

Les workflows peuvent être manuels (checklists), semi-automatisés, ou 100% automatisés.

## 3. Déclencheur (trigger)

Un déclencheur est **l’événement initial** qui lance un processus automatisé :
- Heure précise (cron, planificateur),
- Action utilisateur (formulaire envoyé),
- Événement système (nouveau fichier dans un dossier),
- Webhook/API.

## 4. Condition (if)

Une condition permet d’exécuter une tâche **uniquement si** une règle est vraie :
- Si le fichier contient plus de 100 lignes…
- Si le champ "client" est vide…
- Si l’heure est après 18h…

## 5. Boucle (loop)

On utilise une boucle pour **répéter une action** :
- Pour chaque ligne d’un tableau…
- Pour chaque e-mail non lu…
- Pour chaque image dans un dossier…

## 6. Exception / Gestion d’erreurs

Toute automatisation bien pensée doit savoir :
- Identifier les **erreurs possibles** (fichier manquant, mauvaise structure),
- Réagir (stopper le processus, envoyer une alerte, passer à l’élément suivant).

## 7. Entrées / Sorties

Un processus automatisé consomme une **entrée** (input) et produit une **sortie** (output) :
- Entrée : fichier, base de données, e-mail reçu,
- Sortie : document, mail envoyé, API appelée, log enregistré.

---

## Ces concepts s’appliquent à tout type d’automatisation

| Concept         | Exemple simple                         |
|-----------------|-----------------------------------------|
| Tâche           | Convertir un fichier PDF en Word        |
| Déclencheur     | Nouveau fichier ajouté dans un dossier  |
| Condition       | Si fichier > 5 Mo, le compresser        |
| Boucle          | Appliquer une règle à chaque ligne      |
| Exception       | Si erreur, envoyer une alerte Slack     |

Comprendre ces briques de base, c’est déjà **savoir modéliser** une automatisation, même sans écrire une ligne de code.


