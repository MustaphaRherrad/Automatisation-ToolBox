# Automatiser des actions sur le web

Il est possible de piloter automatiquement un navigateur web pour **remplir des formulaires, cliquer sur des boutons, naviguer sur des sites**, etc. Cela permet d'automatiser des tâches manuelles en ligne.

## 1. Pourquoi automatiser le web ?

- Automatiser la saisie de données dans un back-office.
- Télécharger des fichiers depuis un espace sécurisé.
- Simuler l’usage d’une application web.

## 2. L’outil principal : Selenium

[Selenium](https://www.selenium.dev/) est une bibliothèque qui permet de piloter un navigateur (Chrome, Firefox, etc.) depuis un script Python.

## 3. Exemple simple

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

navigateur = webdriver.Chrome()
navigateur.get("https://exemple.com")

# Remplir un formulaire
champ = navigateur.find_element(By.NAME, "email")
champ.send_keys("adresse@exemple.com")

# Cliquer sur un bouton
bouton = navigateur.find_element(By.ID, "valider")
bouton.click()
````

## 4. Attendre le chargement

Pour éviter les erreurs, on peut attendre qu’un élément soit visible :

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

WebDriverWait(navigateur, 10).until(
    EC.presence_of_element_located((By.ID, "valider"))
)
```

## 5. Enregistrer un script (bonus)

L’extension Chrome **Selenium IDE** permet d’enregistrer automatiquement une navigation pour générer un script.

---

**Attention :**

* Certains sites bloquent les robots (Cloudflare, captchas, etc.).
* Ne pas utiliser pour contourner des protections.

---

**L'automatisation du web est l’ultime recours quand il n’existe ni API, ni accès direct aux données.**