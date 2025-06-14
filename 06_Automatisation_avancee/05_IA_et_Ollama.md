Ollama est un excellent outil pour exécuter des modèles de langage locaux (LLM) directement sur votre machine. Voici comment l'installer et l'exploiter dans une automatisation de tâches.

## 1\. Installer Ollama en local

L'installation d'Ollama est simple et dépend de votre système d'exploitation.

### Sur Windows

1.  **Téléchargez l'exécutable :** Rendez-vous sur le site officiel d'Ollama ([ollama.com](https://ollama.com/)) et cliquez sur "Download for Windows".
2.  **Exécutez l'installateur :** Ouvrez le fichier `.exe` téléchargé et suivez les instructions. L'installation est généralement très simple, comme pour un logiciel standard.
3.  **Vérifiez l'installation :** Ouvrez PowerShell ou l'Invite de commandes et tapez `ollama --version`. Si vous voyez un numéro de version, Ollama est bien installé.

### Sur Linux

1.  **Ouvrez un terminal :**
2.  **Exécutez la commande d'installation :** Copiez et collez la commande suivante dans votre terminal :
    ```bash
    curl -fsSL https://ollama.com/install.sh | sh
    ```
    Cette commande télécharge et exécute un script d'installation.
3.  **Vérifiez l'installation :** Dans le terminal, tapez `ollama --version`. Si vous voyez un numéro de version, c'est bon.

### Sur macOS

1.  **Téléchargez l'application :** Allez sur [ollama.com](https://ollama.com/) et cliquez sur "Download for macOS".
2.  **Installez l'application :** Faites glisser le fichier `.dmg` dans votre dossier "Applications".
3.  **Lancez Ollama :** Ouvrez le Finder, allez dans Applications et double-cliquez sur l'icône Ollama. Un terminal peut s'ouvrir en arrière-plan, c'est normal.
4.  **Vérifiez l'installation :** Ouvrez le Terminal et tapez `ollama --version`. Un numéro de version devrait s'afficher.

### Télécharger un modèle

Une fois Ollama installé, vous devez télécharger un modèle de langage pour l'utiliser. Par exemple, pour télécharger Llama 3 (un modèle populaire) :

```bash
ollama run llama3
```

La première fois que vous exécutez cette commande, Ollama téléchargera le modèle (cela peut prendre un certain temps selon votre connexion internet). Une fois le téléchargement terminé, vous entrerez dans une session de chat avec le modèle.

Vous pouvez également simplement `pull` un modèle sans le lancer immédiatement :

```bash
ollama pull llama3
```

Pour voir la liste des modèles disponibles, visitez la page des modèles sur le site d'Ollama.

## 2\. Exploiter Ollama dans une automatisation de tâches

Ollama peut être utilisé de plusieurs manières pour l'automatisation, principalement via son API ou des bibliothèques clientes. Le plus courant est d'utiliser la bibliothèque Python.

### Utilisation de l'API REST d'Ollama (curl ou requêtes HTTP)

Ollama expose une API REST que vous pouvez interroger. C'est utile pour intégrer Ollama dans n'importe quel langage de programmation ou outil qui peut faire des requêtes HTTP.

**Exemple de génération de texte avec `curl`:**

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3",
  "prompt": "Explique-moi la photosynthèse simplement."
}'
```

**Exemple de chat conversationnel avec `curl`:**

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "llama3",
  "messages": [
    {
      "role": "system",
      "content": "Tu es un assistant utile."
    },
    {
      "role": "user",
      "content": "Quelle est la capitale de la France ?"
    }
  ],
  "stream": false
}'
```

### Utilisation de la bibliothèque Python Ollama

C'est la méthode la plus courante et la plus simple pour intégrer Ollama dans des scripts d'automatisation.

1.  **Installez la bibliothèque Python d'Ollama :**

    ```bash
    pip install ollama
    ```

2.  **Exemple de script Python pour générer du texte :**

    ```python
    import ollama

    def generer_reponse(prompt, model="llama3"):
        try:
            response = ollama.generate(model=model, prompt=prompt)
            return response['response']
        except Exception as e:
            return f"Erreur lors de la génération de la réponse : {e}"

    if __name__ == "__main__":
        question = "Rédige un court paragraphe sur l'importance des énergies renouvelables."
        reponse_ia = generer_reponse(question)
        print(f"Question : {question}\n")
        print(f"Réponse de l'IA : {reponse_ia}")

        # Exemple de chat conversationnel
        messages = [
            {'role': 'user', 'content': 'Quelle est la couleur du ciel ?'},
            {'role': 'assistant', 'content': 'Le ciel est généralement bleu.'},
            {'role': 'user', 'content': 'Pourquoi est-il bleu ?'}
        ]
        chat_response = ollama.chat(model='llama3', messages=messages)
        print(f"\nRéponse du chat : {chat_response['message']['content']}")
    ```

3.  **Exemple d'utilisation pour l'automatisation de tâches :**

    Imaginez que vous vouliez automatiser la rédaction de résumés de textes ou la génération de réponses pour un chatbot.

    ```python
    import ollama

    def resumer_texte(texte, model="llama3"):
        prompt = f"Résume le texte suivant en quelques phrases : \n\n{texte}"
        return ollama.generate(model=model, prompt=prompt)['response']

    def generer_reponse_email(sujet_email, corps_email, model="llama3"):
        prompt = f"Rédige une réponse amicale et professionnelle à l'e-mail suivant. Sujet : '{sujet_email}'. Corps : '{corps_email}'"
        return ollama.generate(model=model, prompt=prompt)['response']

    if __name__ == "__main__":
        long_texte = """
        L'intelligence artificielle (IA) est un domaine en pleine croissance de l'informatique qui se concentre sur la création de machines capables de raisonner, d'apprendre et d'agir de manière autonome. Elle englobe diverses approches, allant de l'apprentissage automatique (machine learning) et de l'apprentissage profond (deep learning) aux systèmes experts et au traitement du langage naturel (NLP). L'IA est en train de transformer de nombreux secteurs, de la santé à la finance en passant par les transports, en offrant des solutions innovantes pour des problèmes complexes. Cependant, son développement soulève également des questions éthiques et sociétales importantes concernant la vie privée, l'emploi et le contrôle.
        """
        resume = resumer_texte(long_texte)
        print(f"Résumé généré : \n{resume}\n")

        email_sujet = "Demande d'informations sur votre nouveau produit"
        email_corps = "Bonjour, je suis très intéressé par votre dernier produit et j'aimerais en savoir plus sur ses fonctionnalités et son prix. Pourriez-vous me fournir des informations supplémentaires ? Cordialement."
        reponse_email = generer_reponse_email(email_sujet, email_corps)
        print(f"Réponse d'email générée : \n{reponse_email}")
    ```

### Intégration avec des outils d'automatisation (Exemple avec des scripts Bash et Cron jobs)

Vous pouvez combiner Ollama avec des scripts shell et des outils de planification comme Cron (sur Linux/macOS) ou le Planificateur de tâches (sur Windows) pour des automatisations plus complexes.

**Exemple de script Bash (pour Linux/macOS) :**

`generate_daily_report.sh`

```bash
#!/bin/bash

# Définir le modèle Ollama à utiliser
MODEL="llama3"

# Prompt pour le rapport
PROMPT="Rédige un rapport concis sur les événements marquants de l'actualité d'hier, en te basant sur les informations générales disponibles."

# Générer le rapport en utilisant l'API Ollama
REPORT=$(curl -s http://localhost:11434/api/generate -d "{
  \"model\": \"$MODEL\",
  \"prompt\": \"$PROMPT\",
  \"stream\": false
}" | jq -r '.response')

# Sauvegarder le rapport dans un fichier
DATE=$(date +"%Y-%m-%d")
echo "$REPORT" > "rapport_quotidien_$DATE.txt"

echo "Rapport quotidien généré et sauvegardé dans rapport_quotidien_$DATE.txt"
```

  * Assurez-vous que `jq` est installé (`sudo apt-get install jq` sur Debian/Ubuntu ou `brew install jq` sur macOS) pour parser la réponse JSON.
  * Rendez le script exécutable : `chmod +x generate_daily_report.sh`

**Planification avec Cron (sur Linux/macOS) :**

Pour exécuter ce script tous les jours à 8h du matin :

1.  Ouvrez votre crontab : `crontab -e`
2.  Ajoutez la ligne suivante à la fin du fichier :
    ```cron
    0 8 * * * /chemin/vers/votre/generate_daily_report.sh
    ```
    Remplacez `/chemin/vers/votre/` par le chemin réel de votre script.

### Points clés pour l'automatisation :

  * **API REST :** L'API d'Ollama est le point d'entrée principal pour l'intégration programmatique.
  * **Bibliothèques clientes :** Utilisez la bibliothèque Python d'Ollama pour une intégration facile dans vos scripts Python. D'autres langages peuvent avoir des bibliothèques ou vous pouvez utiliser des requêtes HTTP directes.
  * **Gestion des modèles :** Assurez-vous que les modèles nécessaires sont téléchargés (`ollama pull <model>`) avant d'exécuter vos automatisations.
  * **Gestion des erreurs :** Incluez des mécanismes de gestion des erreurs dans vos scripts pour traiter les problèmes de connexion à Ollama ou les réponses inattendues.
  * **Performance :** Les LLM peuvent être gourmands en ressources. Assurez-vous que votre machine dispose de suffisamment de RAM et d'un GPU si vous utilisez des modèles plus grands pour de meilleures performances.
  * **Contexte :** Pour les interactions conversationnelles ou les tâches nécessitant une mémoire, utilisez la fonction `ollama.chat` et gérez la liste des messages pour maintenir le contexte.
  * **Paramètres du modèle :** Explorez les options de génération comme `temperature` (créativité), `top_k`, `top_p` et `num_predict` pour affiner les réponses du modèle selon vos besoins d'automatisation.

En suivant ces étapes, vous pourrez installer Ollama en local et l'intégrer efficacement dans vos processus d'automatisation pour tirer parti de la puissance des grands modèles de langage.