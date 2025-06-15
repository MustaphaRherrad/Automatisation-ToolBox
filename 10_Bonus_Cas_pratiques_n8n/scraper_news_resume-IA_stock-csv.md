Voici un exemple **simple et gratuit** avec n8n pour :  
**1.** Scraper un flux RSS (ou un site), **2.** Envoyer le texte à une IA gratuite (LLM) pour résumé, **3.** Stocker les résultats dans un fichier CSV local.  

---

### **Workflow n8n : Résumé automatisé d'actualités → CSV**
#### **Outils gratuits utilisés** :
- **Source** : Flux RSS (ex : [BBC News](https://feeds.bbci.co.uk/news/rss.xml)) ou API gratuite (ex : [NewsAPI](https://newsapi.org/free-tier)).
- **IA** : OpenAI GPT-3.5 (gratuit via [Ollama](https://ollama.ai/) en local) ou [Hugging Face Inference API](https://huggingface.co/inference-api) (gratuit pour les petits volumes).
- **Stockage** : Fichier CSV sur ton PC (via n8n).

---

### **Étapes détaillées**

#### **1. Récupérer les actualités (Flux RSS)**
- Ajoute un nœud **"RSS Feed Read"** (ou **"HTTP Request"** pour un site sans RSS).
  - *Exemple avec BBC RSS* :
    - URL : `https://feeds.bbci.co.uk/news/rss.xml`
    - Options : Limite à 5 articles pour les tests.

#### **2. Filtrer par mots-clés (Optionnel)**
- Ajoute un nœud **"Function"** ou **"IF"** pour filtrer les titres/contenus :
  ```javascript
  // Code pour le nœud "Function"
  const keywords = ["technologie", "IA", "science"];
  const title = $input.all()[0].json.title.toLowerCase();
  const matches = keywords.some(word => title.includes(word));

  if (matches) return $input.all()[0];
  else return null;
  ```

#### **3. Envoyer à une IA pour résumé**
- **Option 1 (Ollama en local)** :
  - Installe [Ollama](https://ollama.ai/) et un modèle (ex: `ollama pull llama3`).
  - Ajoute un nœud **"HTTP Request"** vers `http://localhost:11434/api/generate` :
    - Méthode : POST
    - Headers : `{"Content-Type": "application/json"}`
    - Body :
      ```json
      {
        "model": "llama3",
        "prompt": "Résume cet article en 2 phrases : " + $input.all()[0].json.description,
        "stream": false
      }
      ```

- **Option 2 (Hugging Face gratuit)** :
  - Crée un compte sur [Hugging Face](https://huggingface.co/) et génère un token API.
  - Utilise le nœud **"Hugging Face"** avec un modèle comme `facebook/bart-large-cnn`.
  - Prompt : `"Résume ceci : " + $input.all()[0].json.description`

#### **4. Formater et sauvegarder en CSV**
- Ajoute un nœud **"Function"** pour structurer les données :
  ```javascript
  return {
    date: new Date().toISOString().split('T')[0], // Date du jour
    titre: $input.all()[0].json.title,
    url: $input.all()[0].json.link,
    résumé: $input.all()[1].json.response || $input.all()[1].json.summary_text,
    catégorie: "technologie" // À adapter ou ajouter un champ dynamique
  };
  ```

- Ajoute un nœud **"Spreadsheet File"** :
  - Opération : "Append to File"
  - Chemin du fichier : `/chemin/vers/ton/fichier.csv` (ex: `~/actualites.csv`)
  - *Assure-toi que n8n a les droits d'écriture sur ton dossier.*

---

### **Exemple de fichier CSV généré**
| date       | titre                     | url          | résumé                          | catégorie   |
|------------|---------------------------|--------------|----------------------------------|-------------|
| 2024-06-14 | Une avancée en IA...      | bbc.com/123  | "L'IA a réalisé..."             | technologie |

---

### **Automatisation**
- **Planification** : Ajoute un nœud **"Cron"** pour exécuter le workflow tous les jours à 9h.
- **Notifications** : Branch un nœud **"Email"** ou **"Telegram"** pour recevoir les résumés.

---

### **Résolution de problèmes**
- **Erreurs HTTP** : Vérifie que les URLs (RSS/API) sont accessibles.
- **Limites d'IA gratuites** : Ollama est illimité en local, mais Hugging Face a un quota (env. 1000 requêtes/mois).
- **Permissions CSV** : Utilise un chemin absolu (ex: `/home/user/documents/actualites.csv`).

---

Ce workflow tourne **100% gratuitement** (hors coût électricité pour Ollama). Tu peux le complexifier en ajoutant des catégories dynamiques ou en sauvegardant dans Google Sheets (via l’intégration gratuite).