# 🌟 **Cours rapide sur les FSMO (Flexible Single Master Operations)**

<details>
<summary>📝 **Qu'est-ce que les FSMO ?**</summary>
Les **FSMO** sont des rôles critiques assignés à certains contrôleurs de domaine (DC) dans un environnement Active Directory. Ces rôles assurent des tâches essentielles qui ne peuvent pas être effectuées en mode multi-maître.  
On distingue **5 rôles FSMO**, répartis en deux catégories :
- 🌐 **Forest-wide roles** (2 rôles pour toute la forêt AD)
- 🏛️ **Domain-wide roles** (3 rôles spécifiques à chaque domaine)
</details>

---

<details>
<summary>🔍 **Les 5 rôles FSMO et leurs fonctions**</summary>

### 🌐 **Forest-wide roles**
1. **Schema Master (Maître de schéma)**  
   🗂️ **Portée** : Forêt  
   🎯 **Rôle** : Responsable des modifications du schéma AD (ajout/modification d’attributs et classes).  

2. **Domain Naming Master (Maître des noms de domaine)**  
   🗂️ **Portée** : Forêt  
   🎯 **Rôle** : Gère l’ajout/suppression de domaines et d’espaces de noms dans la forêt.  

### 🏛️ **Domain-wide roles**
3. **RID Master (Relative ID Master)**  
   🗂️ **Portée** : Domaine  
   🎯 **Rôle** : Attribue des pools de RID pour garantir des SID uniques.  

4. **PDC Emulator (Émulateur PDC)**  
   🗂️ **Portée** : Domaine  
   🎯 **Rôle** :  
   - Compatibilité avec les anciens systèmes Windows NT.  
   - Gestion des mots de passe.  
   - Source de temps pour le domaine.  

5. **Infrastructure Master**  
   🗂️ **Portée** : Domaine  
   🎯 **Rôle** : Gère les références inter-domaines et met à jour les liens vers des objets d'autres domaines.  
   ⚠️ **Attention** : Ne doit pas être sur un DC qui est aussi Global Catalog (GC), sauf si tous les DC sont GC.
</details>

---

<details>
<summary>💡 **Pourquoi les FSMO sont-ils importants ?**</summary>
Les rôles FSMO centralisent certaines tâches critiques pour éviter les conflits et assurer le bon fonctionnement d'Active Directory.  
Exemples :
- Gérer les modifications de schéma.
- Assurer la synchronisation des identités.
- Maintenir la compatibilité avec d'anciens systèmes.
</details>

---

<details>
<summary>⚙️ **Vérification et gestion des FSMO**</summary>

### 🔍 **1. Vérifier les rôles FSMO**
- **Commande PowerShell** :
  ```powershell
  Get-ADForest | Select-Object SchemaMaster, DomainNamingMaster
  Get-ADDomain | Select-Object RIDMaster, PDCEmulator, InfrastructureMaster
  ```
- **Commande `netdom`** :
  ```cmd
  netdom query fsmo
  ```

### 🔧 **2. Transférer ou saisir un rôle FSMO**
- **Transfert via outils GUI** :  
  - AD Users and Computers  
  - AD Domains and Trusts  
- **Commande PowerShell ou `ntdsutil`** pour le transfert ou la saisie forcée en cas de panne.

</details>

---

### 📌 **Résumé des rôles :**
| 🛠️ **Rôle**              | 🌍 **Portée**  | 📋 **Fonction principale**                          |
|--------------------------|---------------|---------------------------------------------------|
| 🗂️ Schema Master         | Forêt         | Gère le schéma AD.                                |
| 🗺️ Domain Naming Master  | Forêt         | Ajoute/supprime des domaines dans la forêt.       |
| 🆔 RID Master            | Domaine       | Attribue des pools de RID pour les objets.        |
| 🕒 PDC Emulator          | Domaine       | Compatibilité NT, mots de passe, source de temps. |
| 🔗 Infrastructure Master | Domaine       | Gère les références inter-domaines.               |
