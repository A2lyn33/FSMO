## **Qu’est-ce que les FSMO ?**

Les **FSMO** sont des rôles spéciaux assignés à certains contrôleurs de domaine (DC) dans un environnement Active Directory (AD). Ces rôles garantissent le bon fonctionnement d'AD en assurant des tâches critiques qui ne peuvent pas être effectuées en mode multi-maître.

Il y a **5 rôles FSMO**, divisés en deux catégories :  
1. **Forest-wide roles** (2 rôles pour toute la forêt AD)  
2. **Domain-wide roles** (3 rôles spécifiques à chaque domaine)

---

## **Les 5 rôles FSMO et leurs fonctions**

### **1. Schema Master (Maître de schéma)**
- **Portée** : Forêt
- **Rôle** : Responsable des modifications du schéma AD (ajout ou modification d’attributs, classes, etc.).
- **Usage** : Un seul DC dans toute la forêt peut modifier le schéma.

---

### **2. Domain Naming Master (Maître des noms de domaine)**
- **Portée** : Forêt
- **Rôle** : Responsable de l’ajout ou de la suppression de domaines et d’espaces de noms dans la forêt.
- **Usage** : Évite les conflits dans les noms de domaines ou d’espaces.

---

### **3. RID Master (Relative ID Master)**
- **Portée** : Domaine
- **Rôle** : Gère les pools de RID (Relative Identifiers) pour les objets dans le domaine.
- **Usage** : Permet aux DC de créer des objets avec des SID (Security Identifiers) uniques.

---

### **4. PDC Emulator (Émulateur PDC)**
- **Portée** : Domaine
- **Rôle** : Fournit la compatibilité avec les anciens systèmes (Windows NT) et centralise certaines opérations :
  - Source de temps pour les autres DC.
  - Gestion des mots de passe (force ou verrouille les changements).
  - Gère les conflits liés à la réplication.
- **Usage** : Souvent le DC principal dans les interactions utilisateur.

---

### **5. Infrastructure Master**
- **Portée** : Domaine
- **Rôle** : Gère les références inter-domaines et met à jour les liens vers les objets d'autres domaines.
- **Attention** : Ne doit pas être hébergé sur un DC qui est aussi **Global Catalog** sauf si tous les DC sont GC.

---

## **Pourquoi sont-ils importants ?**
Les rôles FSMO centralisent certaines tâches critiques pour éviter les conflits dans un environnement Active Directory décentralisé.

---

## **Vérification et gestion des FSMO**
### **1. Vérifier les rôles FSMO :**
- Commande PowerShell :
  ```powershell
  Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster
  Get-ADDomain | Select-Object RIDMaster, PDCEmulator, InfrastructureMaster
  ```
- Commande `netdom` :
  ```cmd
  netdom query fsmo
  ```

### **2. Transférer ou saisir un rôle FSMO :**
- Transfert via outils GUI (AD Users and Computers, AD Domains and Trusts).
- Transfert ou saisie forcée via PowerShell ou commande `ntdsutil` en cas de panne.

---

## **Résumé rapide des rôles :**
| **Rôle**               | **Portée**  | **Fonction principale**                              |
|------------------------|------------|-----------------------------------------------------|
| Schema Master          | Forêt      | Gère le schéma AD.                                  |
| Domain Naming Master   | Forêt      | Ajoute/supprime des domaines dans la forêt.         |
| RID Master             | Domaine    | Attribue des pools de RID pour les objets.          |
| PDC Emulator           | Domaine    | Compatibilité NT, mots de passe, source de temps.   |
| Infrastructure Master  | Domaine    | Gère les références inter-domaines.                 |

---

Les FSMO sont essentiels pour un fonctionnement optimal d'Active Directory. 😊
