# Gestion des erreurs dans les scripts d'automatisation

Une automatisation robuste sait comment réagir en cas de problème. Sans gestion des erreurs, un script peut s’arrêter brutalement ou provoquer des conséquences inattendues.

## 1. Types d’erreurs courantes

- **Erreur de syntaxe** : fautes de frappe ou de structure.
- **Erreur de logique** : le script ne fait pas ce qu’on attend.
- **Erreur d’exécution** : fichier manquant, serveur indisponible, division par zéro…

## 2. Gérer les erreurs en Python (exemple)

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Division par zéro interdite.")
except Exception as e:
    print(f"Erreur inattendue : {e}")
finally:
    print("Script terminé.")
````

* `try` : le bloc que l’on teste.
* `except` : ce qu’on fait si une erreur est détectée.
* `finally` : s’exécute dans tous les cas (utile pour fermer un fichier, par exemple).

## 3. Journalisation (logging)

Pour identifier la cause d’une erreur, il faut **enregistrer les événements**.

```python
import logging
logging.basicConfig(filename='log.txt', level=logging.INFO)
logging.error("Une erreur s’est produite")
```

Utiliser des logs permet aussi de savoir :

* Quand le script a été lancé.
* Quelles étapes ont été réalisées.
* À quelle étape il s’est arrêté.

## 4. Alertes et notifications

En cas d’erreur, tu peux être averti par :

* Email (`smtplib` ou via un outil comme SendGrid).
* Slack / Discord (via webhook).
* SMS (via Twilio).

### Exemple :

```python
if erreur_detectee:
    envoyer_email("Erreur survenue dans le script X")
```

## 5. Reprise après échec

* Sauvegarder l’état d’avancement dans un fichier.
* Reprendre à l’étape suivante si le script plante.
* Exécuter une tentative de relance après un échec.

---

## En résumé

| Technique           | Pourquoi l’utiliser             |
| ------------------- | ------------------------------- |
| `try/except`        | Capturer les erreurs            |
| Logging             | Suivre l’exécution              |
| Alertes             | Être notifié en cas de problème |
| `finally`           | Nettoyer les ressources         |
| Relance automatique | Reprendre après interruption    |

**Une bonne gestion des erreurs = un automatisme fiable.**