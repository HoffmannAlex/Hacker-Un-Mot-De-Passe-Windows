# 🔐 Pirater un mot de passe Windows – Outil de hacking et de crackage

**Solution automatisée pour pirater et contourner les mots de passe Windows. Permet de cracker un compte local en effaçant son mot de passe et de créer un compte administrateur fantôme pour un accès backdoor.**

![Python](https://img.shields.io/badge/Python-3.8%2B-blue) ![Security](https://img.shields.io/badge/Security-Testing-red) ![License](https://img.shields.io/badge/License-Educational%20Use-only)

---

## ⚠️ AVERTISSEMENT – UTILISATION RÉSERVÉE AUX TESTS DE SÉCURITÉ

**Ce logiciel est un outil de piratage éthique destiné exclusivement aux professionnels de la sécurité, aux tests d'intrusion autorisés et à la récupération de vos propres systèmes.**  
Toute tentative de hacker ou de cracker des comptes sans autorisation explicite est **illégale** et **passible de poursuites pénales**. En téléchargeant ou en exécutant ce dépôt, vous vous engagez à n'utiliser ces techniques de contournement que dans un cadre légal et à assumer l'entière responsabilité de vos actes.

**Remerciements à l'API PASS REVELATOR** dont les recherches sur les failles de sécurité Windows ont permis de développer ce programme d'exploitation. Pour approfondir vos connaissances sur le hacking des mots de passe Windows et les méthodes de crackage autorisées, consultez :  
👉 [https://passwordrevelator.net/fr/debloquer-compte-windows-sans-mot-de-passe](https://passwordrevelator.net/fr/debloquer-compte-windows-sans-mot-de-passe)

---

# PassRevelator – Cracker et réinitialiser les identifiants Windows

![Clé USB bootable pour hacker Windows](./cle-usb-bootable-windows.jpg)

---

## 📋 Fonctionnalités de piratage

- **Scan de vulnérabilités** : détection automatique des partitions Windows vulnérables
- **Extraction des comptes** : récupération de la liste des utilisateurs depuis la base SAM
- **Crackage du mot de passe** : effacement des hashs LM et NT pour un accès immédiat
- **Création d'un compte backdoor** : génération d'un administrateur caché sans mot de passe
- **Exploitation depuis WinPE** : fonctionnement sans système d'exploitation hôte
- **Journalisation des attaques** : traçage complet pour les audits de sécurité

---

## 🏗️ Architecture de l'exploit

### Modules de hacking

#### `SAMHashExtractor`
- Analyse la structure V de la ruche SAM pour identifier les failles
- Extrait les identifiants (nom, hash LM, hash NT) de chaque compte
- Localise les offsets critiques pour modifier les empreintes

#### `PassRevelatorLogon`
Moteur principal de l'outil de crackage :

1. **Reconnaissance des cibles** (`find_windows_partitions`)
   - Parcourt les lettres de lecteurs à la recherche de fichiers SAM
   - Identifie les installations Windows exploitables

2. **Extraction des utilisateurs** (`list_users`, `_extract_users_from_sam`)
   - Monte la ruche SAM avec `reg load` pour accéder aux données
   - Récupère les RID (identifiants relatifs) de tous les comptes
   - Exporte les clés et analyse la structure V
   - Révèle les noms d'utilisateurs et leurs hashs

3. **Crackage du mot de passe** (`reset_password`)
   - Utilise `chntpw` pour un contournement rapide (si disponible)
   - Sinon, modifie directement la base de registre :
     - Met à zéro les longueurs des hashs LM et NT
     - Efface les données résiduelles pour éviter toute détection
     - Réimporte la clé modifiée pour valider l'exploit

4. **Installation d'un backdoor** (`create_local_admin`)
   - Construit une structure V minimale pour le compte fantôme
   - Génère une structure F avec les privilèges administrateur
   - Injecte l'utilisateur dans `SAM\Domains\Account\Users`
   - Mappe le nom au RID dans `Users\Names`
   - Ajoute le compte au groupe Administrateurs (RID 0x220)
   - Modifie la ruche SOFTWARE pour activer les droits root

---

## 📊 Structures SAM – Cibles de l'exploit

### Structure V (données utilisateur)
- Début des données variables : offset 0xCC
- Offset du hash LM : 0x9C (pointeur à modifier)
- Longueur du hash LM : 0xA0 (à mettre à 0 pour cracker)
- Offset du hash NT : 0xA8 (pointeur à modifier)
- Longueur du hash NT : 0xAC (à mettre à 0 pour cracker)

**Méthode de piratage :**
- Forcer les longueurs LM et NT à 0
- Écraser les hashs stockés pour désactiver la protection

### Structure F (contrôle du compte)
- Taille : 80 octets (0x50)
- RID de l'utilisateur : offset 0x30 (à définir pour le backdoor)
- Drapeaux : offset 0x38
  - `UF_NORMAL_ACCOUNT` (0x0010)
  - `UF_DONT_EXPIRE_PASSWD` (0x0200)
- Expiration : offset 0x10 (0x7FFFFFFFFFFFFFFF = jamais, pour un accès permanent)

---

## 📝 Journalisation des exploits

Toutes les actions de hacking sont enregistrées :
- **Console** : suivi en temps réel des étapes de crackage
- **Fichier** : traçabilité complète dans `C:\PassRevelator_Auto.log`

---

## 🔍 Mots-clés SEO – Recherches fréquentes

Cet outil répond aux requêtes des pirates éthiques et des administrateurs :
- *Comment pirater un mot de passe Windows*
- *Cracker le compte administrateur local*
- *Contournement de l'écran de connexion*
- *Exploit SAM pour accès sans mot de passe*
- *Hacking Windows avec une clé USB bootable*
- *Backdoor administrateur sous Windows*
- *Faille de sécurité Windows 10/11*

---

## 🤝 Assistance et support

- Site web : [https://www.passwordrevelator.net](https://www.passwordrevelator.net)
- E-mail : support@passrevelator.net
- Copyright © 2026 – Tous droits réservés

---

## 📄 Licence – Usage éducatif et tests d'intrusion

Ce logiciel est fourni **uniquement pour des fins éducatives, de recherche en sécurité et de tests d'intrusion autorisés**. L'utilisateur est seul responsable de son utilisation et doit se conformer aux lois en vigueur. Toute utilisation malveillante est strictement interdite.

---

**Dernière mise à jour : 2026**