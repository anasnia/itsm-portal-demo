# Runbook Admin — Portail ITSM (Jira Service Management)

> Objectif : ce document est le **mode d’emploi** du portail IT.
> Il explique **qui peut modifier la configuration**, **comment on fait des changements sans casser**, **comment on nomme les choses**, et **comment on pilote au quotidien** (SLA/KPI/KB).
>
> Pourquoi c’est important : sans règles, l’outil devient vite un bazar (champs en doublon, workflows incohérents, SLA non respectés, stats inutilisables).

---

## 0) TL;DR (version très simple)
- Un **admin** a les “clés” (workflow, SLA, automations, permissions).
- On ne change **jamais** un truc en prod “au feeling” : on **teste**, on **déploie**, on **note**.
- On garde des **noms standards** (sinon personne ne comprend et les KPI deviennent faux).
- On a des **routines** : triage, revue SLA, revue KB.
- Portfolio : **pas de données sensibles**, pas de tokens, screenshots anonymisés.

---

## 1) Qui fait quoi ? (avec exemples concrets)
### Admin (Tooling / JSM Admin)
**Responsable de la configuration** :
- Workflows & statuts
- Champs & écrans
- Automations
- SLA
- Permissions / rôles
- Dashboards (structure)

**Exemple :**
Un agent dit : “J’aimerais ajouter un statut `Waiting for vendor`”.
→ L’admin analyse l’impact (SLA, reporting), teste, puis ajoute (ou refuse si ça complique trop).

### Service Desk Lead (responsable support)
**Responsable des règles métier** (comment le support travaille) :
- Qu’est-ce qu’un P1/P2
- Quelles priorités / quels SLA
- Quelles catégories
- Qui approuve quoi

**Exemple :**
“Demande d’accès Admin = approbation manager obligatoire.”
→ Le lead valide la règle, l’admin l’implémente.

### Agents (Support N1/N2)
**Utilisateurs opérateurs** :
- qualifient les tickets (catégorie, priorité)
- résolvent / escaladent
- alimentent la base de connaissance

**Exemple :**
Après 10 tickets “VPN Mac”, un agent écrit une KB “VPN Mac — checklist”.

### Approvers (managers)
**Valident** certaines demandes :
- Accès application
- Matériel
- Onboarding

**Exemple :**
“Je veux accès à l’ERP en écriture.”
→ le manager approuve ou refuse.

---

## 2) Règles d’or (pour éviter le chaos)
### Règle 1 — 1 besoin = 1 ticket = 1 catégorie
**Exemple :**
“VPN + j’ai besoin d’un nouvel écran” → 2 tickets (incident + demande matériel).
Sinon, impossible de mesurer le support correctement.

### Règle 2 — Pas de changements en prod sans test
**Exemple :**
Tu modifies une automation qui auto-close les tickets.
Si tu te trompes, tu peux clôturer des tickets critiques automatiquement.

### Règle 3 — Chaque changement laisse une trace
Sinon personne ne sait “pourquoi ça marche plus”.

---

## 3) Change management (comment on modifie la config sans casser)
### Process simple (V1)
1) **Demande** : on écrit ce qu’on veut changer (1 phrase + objectif)
2) **Impact** : est-ce que ça touche workflow, SLA, reporting, permissions ?
3) **Test** : on teste sur un projet de test (ou sur tickets fictifs)
4) **Déploiement** : on met en prod
5) **Log** : on note le changement

### Exemple réel de changement
**Besoin :** “Auto-assigner les incidents VPN au groupe Réseau.”
- Demande : “routing automatique pour Incident - VPN”
- Test : créer un ticket “Incident - VPN” fictif → vérifier l’assignation
- Prod : activer la règle
- Log : noter date + règle + auteur

### Log de changements (modèle)
| Date | Changement | Pourquoi | Fait par | Risque / rollback |
|------|------------|----------|----------|------------------|
| 2025-..-.. | Auto-assign Incident VPN → IT Network | Réduire délai triage | Admin | Désactiver la rule |

---

## 4) Conventions de nommage (ce qui fait “pro”)
Sans conventions : tu obtiens des champs du type “App”, “Application”, “Appli”, “application_name”… et plus rien n’est fiable.

### Request types (standard)
- `Incident - VPN`
- `Request - Access`
- `Request - Hardware`
- `Request - Onboarding`

### Champs (simples et cohérents)
- `Impact` (1-3)
- `Urgency` (1-3)
- `Application` (liste)
- `Access type` (Read/Write/Admin)
- `Manager` (approver)

### Catégories (baseline)
- Réseau (VPN/Wi-Fi)
- Poste de travail (PC/OS)
- Applications (ERP/Jira/Office)
- Matériel (Laptop/Monitor)
- Sécurité (MFA/permissions)

**Exemple :**
Si on écrit parfois “VPN” et parfois “Réseau/VPN”, les stats deviennent fausses.
→ On choisit un standard et on s’y tient.

---

## 5) Exploitation au quotidien (les routines)
### A) Triage (2 fois / jour, 10 minutes)
Checklist :
- Le ticket est assigné à la bonne équipe ?
- La priorité est cohérente ?
- Il manque une info (OS, erreur exacte, capture écran) ?
- Le statut est correct ?

**Exemple :**
Ticket “VPN marche pas” → on demande OS + message d’erreur, sinon on perd 2 jours.

### B) Revue SLA (hebdo, 20 minutes)
- Lister les tickets SLA breached
- Chercher 1 cause principale
- Prendre 1 action simple

**Exemple :**
Cause : trop de tickets restent en `New` sans assignation.
Action : automation auto-assign + discipline triage.

### C) Knowledge Base (mensuel)
- Ajouter un article pour tout problème récurrent
- Mettre à jour les procédures obsolètes

**Exemple :**
Nouvelle version du VPN → KB “VPN diagnostic” mise à jour.

---

## 6) Standards “data quality” (pour que les KPI soient fiables)
On veut éviter :
- tickets sans catégorie
- tickets sans impact/urgence
- tickets fermés sans résolution

### Règles simples
- Impossible de passer à `Done` sans champ `Resolution`
- Impact/Urgence obligatoires sur Incident - VPN
- Catégorie obligatoire sur tout ticket

---

## 7) Sécurité & portfolio (important pour GitHub)
### Ce qu’on ne publie jamais
- tokens API, webhooks secrets, mots de passe
- données réelles (noms, emails, nom d’entreprise)

### Ce qu’on fait à la place
- données fictives : “Jean Dupont”, “ACME”
- floutage des emails/URL dans les screenshots

---

## 8) Definition of Done (pour dire “c’est propre”)
Le portail est “bien administré” quand :
- les workflows sont simples et stables
- les SLA tournent et sont suivis
- les dashboards répondent à “où ça bloque ?”
- la KB existe et est utilisée
- chaque changement est traçable

- ---

## FAQ (questions qu’on se pose toujours en vrai projet)

### 1) “Pourquoi on ne rajoute pas 15 statuts dans le workflow ?”
Parce que plus tu ajoutes de statuts, plus tu crées :
- des erreurs humaines (les gens mettent le mauvais statut),
- des SLA/reportings incohérents,
- des réunions pour expliquer “c’est quoi la différence entre Waiting X et Waiting Y”.

✅ Bonne pratique : commencer simple (6 statuts max) et n’ajouter un statut QUE s’il apporte une valeur mesurable.

---

### 2) “Quand est-ce qu’on fait un nouveau champ ?”
Seulement si :
- le champ est utile à une décision (routage/priorité/approbation),
- ou utile à un KPI (pilotage),
- ou obligatoire pour résoudre (info technique).

**Exemple OK :** `OS` sur Incident VPN (utile pour diagnostiquer).  
**Exemple inutile :** `Couleur préférée` (aucune valeur support).

---

### 3) “Pourquoi on impose Impact/Urgency ?”
Parce que sinon la priorité est au feeling → le support devient injuste et imprévisible.

**Exemple :**
- Un agent met tout en P1 “pour aller vite” → chaos.
- Avec Impact/Urgency : la priorité est défendable et stable.

---

### 4) “Pourquoi on force une ‘Resolution’ avant Done ?”
Pour éviter les clôtures “Done” sans explication.
Ça aide :
- l’utilisateur (il comprend),
- le support (historique),
- la KB (on transforme une résolution en article si récurrent).

---

### 5) “C’est quoi un bon dashboard ?”
Un dashboard est bon si en 2 minutes tu peux répondre :
- “On est sous l’eau ou ça va ?” (backlog)
- “Où on est en retard ?” (SLA)
- “Qu’est-ce qui génère le plus de demandes ?” (volumes/types)
- “Qu’est-ce qui prend le plus de temps ?” (résolution)

---

## “Common mistakes” (erreurs fréquentes et comment les éviter)

### Erreur A — Tout le monde a les droits admin
Conséquence : modifications non maîtrisées → bugs → personne responsable.  
✅ Fix : rôles clairs + seuls admins modifient config.

### Erreur B — Trop de champs / trop tôt
Conséquence : les gens ne remplissent pas → données pourries → KPI inutiles.  
✅ Fix : “minimum fields” au départ, enrichissement progressif.

### Erreur C — Pas de log de changements
Conséquence : “ça marchait hier” → enquête interminable.  
✅ Fix : un tableau de log simple suffit.

---

## Release notes (mini format)
À utiliser après toute modification de config.

**Date :** YYYY-MM-DD  
**Changement :** (1 phrase)  
**Pourquoi :** (objectif)  
**Impact :** (agents / users / SLA / reporting)  
**Rollback :** (comment revenir en arrière)


