# 04 Infrastructure as code
L'Infrastructure as Code (IaC) est une pratique qui consiste à gérer et à provisionner l'infrastructure informatique (serveurs, réseaux, bases de données, etc.) à l'aide de fichiers de configuration lisibles par une machine, plutôt que de le faire manuellement. Cela permet d'automatiser le déploiement, la gestion et la mise à l'échelle de l'infrastructure, en la traitant de la même manière que le code applicatif.

### Pourquoi l'Infrastructure as Code est Cruciale en Automatisation ?

L'IaC apporte des avantages considérables, surtout quand vos automatisations s'exécutent sur des environnements complexes ou doivent être déployées de manière répétée :

* **Cohérence et Évitment du "Drift" :** Garantit que chaque environnement (développement, test, production) est identique, réduisant les erreurs dues à des configurations manuelles incohérentes.
* **Rapidité et Efficacité :** Déploie des infrastructures entières en quelques minutes, sans intervention humaine.
* **Reproducibilité :** Crée de nouveaux environnements à l'identique, ce qui est idéal pour les tests ou la reprise après sinistre.
* **Versionnement :** Comme le code, l'infrastructure est versionnée, permettant de suivre les changements, de revenir à des versions précédentes et de collaborer.
* **Réduction des Erreurs Humaines :** Minimise les fautes de frappe ou les oublis liés aux configurations manuelles.
* **Documentation Implicite :** Le code lui-même sert de documentation de l'infrastructure.

### Concepts Clés de l'IaC

* **Déclaratif vs. Impératif :**
    * **Déclaratif :** Vous décrivez l'état final désiré de l'infrastructure, et l'outil IaC détermine comment y parvenir (ex: Terraform, CloudFormation).
    * **Impératif :** Vous spécifiez les étapes exactes pour configurer l'infrastructure (ex: Ansible, Chef, Puppet).
* **Idempotence :** L'exécution répétée du même code IaC doit aboutir au même état final, sans effets secondaires indésirables si l'état est déjà atteint. C'est fondamental pour la fiabilité de l'automatisation.
* **État (State) :** De nombreux outils IaC maintiennent un fichier d'état qui suit l'infrastructure gérée. Cela leur permet de savoir ce qui existe déjà et d'effectuer des mises à jour incrémentielles.
* **Fournisseurs (Providers) :** Les outils IaC interagissent avec différentes plateformes cloud (AWS, Azure, GCP), des hyperviseurs (VMware), des services SaaS via des "fournisseurs" spécifiques.

```mermaid
graph TD
    A[Développeur / Ingénieur] --> B(Écrit Fichiers IaC);
    B -- Versionnement (Git) --> C[Dépôt de Code];
    C --> D[Outil IaC (ex: Terraform)];
    D -- Provisionne / Configure --> E[Infrastructure Cloud];
    E -- (Serveurs, Réseaux, BD) --> F[Application Déployée];
```
*Figure 13 : Cycle de vie de l'Infrastructure as Code*

### Outils d'Infrastructure as Code Populaires

#### 1. Terraform (HashiCorp)

**Terraform** est l'un des outils IaC déclaratifs les plus populaires. Il permet de provisionner et de gérer des infrastructures sur un large éventail de fournisseurs cloud et de services.

* **Points forts :**
    * **Multi-Cloud / Multi-Provider :** Supporte AWS, Azure, GCP, VMware, Kubernetes, Docker, et bien d'autres via un vaste écosystème de fournisseurs.
    * **Syntaxe HCL (HashiCorp Configuration Language) :** Facile à lire et à écrire.
    * **Planification :** La commande `terraform plan` montre exactement les changements qui seront appliqués avant de les exécuter (`terraform apply`).
    * **Gestion de l'État :** Maintient un fichier d'état pour suivre les ressources déployées.
* **Quand l'utiliser :** Pour provisionner des infrastructures complètes (VMs, réseaux, bases de données, load balancers) et les gérer sur leur cycle de vie.

#### 2. Ansible

**Ansible** est un outil IaC impératif, mais aussi très utilisé pour la gestion de configuration et l'automatisation des tâches opérationnelles. Il est sans agent (ne nécessite pas de logiciel installé sur les machines cibles).

* **Points forts :**
    * **Simple et Sans Agent :** Utilise SSH pour Linux/Unix et WinRM pour Windows.
    * **Playbooks (YAML) :** Les configurations sont écrites en YAML, très lisible.
    * **Gestion de Configuration :** Idéal pour installer des logiciels, configurer des services, gérer des fichiers sur des serveurs existants.
    * **Idempotence :** Les modules Ansible sont conçus pour être idempotents.
* **Quand l'utiliser :** Pour configurer des serveurs après leur provisionnement (ex: installer un serveur web, configurer des permissions), déployer des applications, automatiser des tâches d'administration système récurrentes.

#### 3. AWS CloudFormation / Azure Resource Manager / Google Cloud Deployment Manager

Ce sont les solutions IaC natives des grands fournisseurs de cloud. Elles sont spécifiques à leur écosystème mais très bien intégrées.

* **Points forts :**
    * **Intégration Profonde :** Accès à toutes les fonctionnalités et services spécifiques du cloud provider.
    * **Gratuit (avec l'utilisation des services) :** Généralement inclus dans l'abonnement cloud.
    * **Gestion des Dépendances :** Gèrent automatiquement l'ordre de création/suppression des ressources.
* **Quand l'utiliser :** Si vous êtes fortement ancré dans un seul fournisseur cloud et souhaitez tirer parti de toutes ses fonctionnalités.

### Rôle de Python dans l'IaC

Bien que les outils IaC aient leur propre langage (HCL, YAML), Python est un excellent complément :

* **Scripts d'Orchestration :** Python peut appeler et orchestrer les outils IaC (ex: lancer des commandes `terraform apply`).
* **Génération de Fichiers IaC :** Pour des configurations complexes et dynamiques, Python peut générer les fichiers HCL ou YAML des outils IaC.
* **Tests IaC :** Écrire des tests en Python pour valider l'infrastructure déployée.
* **SDKs Cloud :** Utiliser les SDKs Python des fournisseurs cloud (`boto3` pour AWS, `azure-sdk-for-python` pour Azure, `google-cloud-python` pour GCP) pour interagir programmation avec l'infrastructure quand un outil IaC générique n'est pas suffisant.

### Exemple : Provisionner un dossier sur une VM distante avec Ansible et Python (Mini-IaC)

Ce n'est pas une infrastructure cloud complète, mais cela illustre l'orchestration Python d'un outil IaC simple.

**Pré-requis :**
1.  **Ansible installé :** `pip install ansible`
2.  **Machine distante ou locale avec SSH activé :** Pour un test rapide, vous pouvez même cibler votre machine locale avec SSH si Ansible est configuré.
3.  **Fichier `hosts` (inventaire Ansible) :**
    ```ini
    # ansible_demo/hosts
    [local]
    localhost ansible_connection=local

    [servers]
    # remplacez par l'IP/hostname de votre machine cible
    # my_remote_server ansible_host=your_server_ip ansible_user=your_username ansible_ssh_private_key_file=/path/to/your/ssh_key
    ```
4.  **Fichier `create_dir_playbook.yml` (Playbook Ansible) :**
    ```yaml
    # ansible_demo/create_dir_playbook.yml
    ---
    - name: Ensure target directory exists
      hosts: all
      become: yes # Exécute avec des privilèges sudo si nécessaire
      tasks:
        - name: Create directory if it does not exist
          ansible.builtin.file:
            path: /tmp/my_automated_folder
            state: directory
            mode: '0755'
            owner: root
            group: root
    ```

**Fichier `run_ansible.py` (Script Python) :**

```python
# ansible_demo/run_ansible.py
import subprocess
import os

def run_ansible_playbook(playbook_path, inventory_path, target_hosts="local"):
    """
    Exécute un playbook Ansible.
    :param playbook_path: Chemin vers le fichier .yml du playbook.
    :param inventory_path: Chemin vers le fichier d'inventaire Ansible (hosts).
    :param target_hosts: Groupe d'hôtes ciblés dans l'inventaire (ex: "local", "servers").
    """
    print(f"--- Exécution du playbook Ansible '{playbook_path}' sur '{target_hosts}' ---")
    
    # Construire la commande Ansible
    command = [
        "ansible-playbook",
        playbook_path,
        "-i", inventory_path,
        "-l", target_hosts # Limiter l'exécution à un groupe spécifique d'hôtes
    ]

    try:
        # Exécuter la commande
        result = subprocess.run(
            command,
            check=True,          # Lève une CalledProcessError si le code de retour est non-zéro
            capture_output=True, # Capture stdout et stderr
            text=True            # Décode la sortie en texte
        )
        print("\n--- Sortie Ansible ---")
        print(result.stdout)
        if result.stderr:
            print("\n--- Erreurs Ansible (Stderr) ---")
            print(result.stderr)
        print("--- Playbook exécuté avec succès ---")
        return True
    except subprocess.CalledProcessError as e:
        print(f"\n--- Erreur lors de l'exécution du playbook ---")
        print(f"Code de retour : {e.returncode}")
        print(f"Sortie standard :\n{e.stdout}")
        print(f"Erreur standard :\n{e.stderr}")
        return False
    except FileNotFoundError:
        print("Erreur: La commande 'ansible-playbook' n'a pas été trouvée. Ansible est-il installé et dans votre PATH ?")
        return False
    except Exception as e:
        print(f"Une erreur inattendue est survenue: {e}")
        return False

# --- Utilisation ---
if __name__ == "__main__":
    current_dir = os.path.dirname(os.path.abspath(__file__))
    ansible_dir = os.path.join(current_dir, "ansible_demo")
    
    playbook = os.path.join(ansible_dir, "create_dir_playbook.yml")
    inventory = os.path.join(ansible_dir, "hosts")

    # Assurez-vous que les fichiers existent avant de lancer
    if not os.path.exists(playbook):
        print(f"Erreur: Playbook non trouvé à {playbook}")
    elif not os.path.exists(inventory):
        print(f"Erreur: Inventaire non trouvé à {inventory}")
    else:
        # Exécutez sur 'local' si vous n'avez pas de VM distante configurée
        run_ansible_playbook(playbook, inventory, target_hosts="local")
        # Si vous avez une VM distante configurée dans l'inventaire, vous pouvez aussi faire:
        # run_ansible_playbook(playbook, inventory, target_hosts="servers")
```

**Pour exécuter cet exemple :**

1.  Créez un dossier `ansible_demo/`.
2.  Créez les fichiers `hosts` et `create_dir_playbook.yml` à l'intérieur.
3.  Créez le fichier `run_ansible.py` dans le dossier parent (ou ajustez les chemins).
4.  Lancez le script Python.
5.  Vérifiez sur votre machine locale (ou la machine distante si configurée) si le dossier `/tmp/my_automated_folder` (ou `C:\temp\my_automated_folder` sous Windows) a été créé par Ansible.

### Tableau Récapitulatif : Outils d'Infrastructure as Code

| Outil             | Approche     | Cas d'Usage Principal                               | Intégration Python | Points Forts                                   |
| :---------------- | :----------- | :-------------------------------------------------- | :----------------- | :--------------------------------------------- |
| **Terraform** | Déclaratif   | Provisionnement d'infrastructure (VMs, réseaux, DBs) | Orchestration, génération de HCL | Multi-Cloud, planification, gestion d'état     |
| **Ansible** | Impératif    | Gestion de configuration, déploiement d'applications | Orchestration, génération de Playbooks | Sans agent, simple, idempotence                |
| **CloudFormation (AWS)** | Déclaratif   | Provisionnement d'infrastructure AWS uniquement       | SDKs (Boto3)       | Intégration native AWS, gestion des dépendances |
| **Python SDKs (Boto3, etc.)** | Impératif (API) | Interactions granulaires avec les services cloud     | Directe            | Contrôle fin, logique complexe, flexibilité   |

---
