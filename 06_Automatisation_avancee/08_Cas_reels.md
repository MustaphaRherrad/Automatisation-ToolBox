# Études de cas réels

Pour conclure ce chapitre, voici des **exemples d’automatisations inspirées de situations réelles**. Elles montrent la diversité des cas d’usage.

---

## 1. Comptabilité : automatisation de factures

**Contexte** : une TPE génère manuellement des factures chaque fin de mois à partir de feuilles Excel.

**Solution** :
- Lecture des fichiers Excel.
- Génération automatique des PDF via `reportlab` ou `pdfkit`.
- Envoi automatique des PDF aux clients.

**Bénéfices** :
- Gain de temps estimé : 3 heures/mois.
- Moins d’erreurs de saisie.

---

## 2. Veille concurrentielle

**Contexte** : un service marketing veut surveiller les prix de concurrents sur 5 sites e-commerce.

**Solution** :
- Script de scraping quotidien.
- Stockage des prix dans une base de données.
- Notification en cas de baisse ou hausse significative.

**Bénéfices** :
- Réactivité stratégique.
- Automatisation silencieuse mais efficace.

---

## 3. RH : suivi des candidatures

**Contexte** : l’équipe RH reçoit des CV par email et les trie manuellement.

**Solution** :
- Script IMAP pour lire les emails.
- Extraction automatique des pièces jointes.
- Classement dans des dossiers selon le poste.

**Bénéfices** :
- Gain de temps de tri.
- Meilleur suivi.

---

## 4. Reporting automatisé

**Contexte** : chaque semaine, un analyste compile les ventes dans un fichier Excel pour un rapport.

**Solution** :
- Script qui agrège les données (par pays, canal).
- Génération de graphiques avec `matplotlib`.
- Export d’un rapport PDF.

**Bénéfices** :
- Rapport généré en 2 minutes au lieu de 2 heures.
- Automatisable via un cron hebdo.

---

**En conclusion :**  
Les meilleures automatisations sont **simples, utiles et maintenables**. Elles ne remplacent pas les humains, mais leur rendent du temps.