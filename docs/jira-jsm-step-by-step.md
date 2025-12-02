# Jira Service Management (JSM) — Guide ultra-débutant (pas à pas)

Ce guide documente une mise en place simple d’un portail ITSM dans Jira Service Management (Cloud) :
- Portail Help Center
- Ticket d’exemple
- Workflow
- SLA
- Automation
- Dashboard

> Objectif : expliquer les bases + montrer la configuration au fur et à mesure, avec des preuves (captures).

---

## 0) Lexique minimum (2 minutes)

- **ITSM** : façon “organisée” de gérer les demandes IT (incidents, demandes, changements).
- **Incident** : “ça ne marche plus” (ex : VPN down).
- **Service Request / Demande** : “je veux quelque chose” (ex : accès, nouveau matériel).
- **Workflow** : étapes par lesquelles passe un ticket (Open → In progress → Done…).
- **SLA** : engagement de délai (ex : réponse < 8h).
- **Automation** : règles automatiques (“si ‘VPN’ dans le titre → assigner à X”).
- **Dashboard** : page avec des chiffres / listes / graphiques sur les tickets.

---

## 1) Créer l’espace JSM “Support”

### Ce qu’on fait
On crée un espace de support IT (projet JSM) qui va servir de “base” à tout le reste.

### Comment le faire (détaillé)
1. Ouvre Jira dans Atlassian.
2. Va sur “Voir plus d’apps Atlassian” si besoin, puis choisis **Jira Service Management**.
3. Clique sur **Essayer / Démarrer** (si c’est proposé).
4. Choisis le modèle **Support informatique (IT support)**.
5. Donne un nom simple : **Support**
6. Valide la création.

💡 À la fin, tu dois voir “Support” dans le menu à gauche, et une clé projet du style **SUP**.

✅ Résultat attendu
- Un espace nommé **Support**
- Navigation JSM visible : Summary / Files d’attente / Rapports / Base de connaissances / etc.

---

## 2) Portail (Help Center) : vérifier qu’il existe et qu’il affiche des options

### Ce qu’on fait
On vérifie que le **Help Center** existe et que l’utilisateur final peut créer une demande.

### Comment le faire (détaillé)
1. Dans le projet **Support**, trouve l’accès au portail (Help Center).
2. Tu dois arriver sur une page type “Welcome to the Help Center” avec des choix comme :
   - Submit a request or incident
   - Ask a question

✅ Résultat attendu
- Le portail est accessible
- On voit au moins 2 entrées de demandes

📸 Preuve (capture)
![](../screenshots/Portail_itsm.png)

---

## 3) Créer une demande / incident depuis le portail

### Ce qu’on fait
On crée une demande depuis le portail pour générer un “vrai” ticket côté agent.

### Comment le faire (détaillé)
1. Depuis le Help Center, clique **Submit a request or incident**.
2. Remplis les champs :
   - **Summary / Titre** : `VPN down (remote)`
   - **Description** : `Cannot connect to VPN since 09:00. Error: Authentication failed. Team blocked.`
3. Envoie / crée la demande.

✅ Résultat attendu
- Un ticket est créé dans le projet Support (clé du type **SUP-1**, **SUP-2**, etc.)

📸 Preuve (capture)
![](../screenshots/create_incident_request.png)

---

## 4) Vérifier le ticket côté agent (vue Jira)

### Ce qu’on fait
On ouvre le ticket créé pour vérifier qu’il existe, qu’il contient bien les infos, et qu’on voit les blocs (SLA, détails, automation…).

### Comment le faire (détaillé)
1. Reviens côté Jira (vue agent).
2. Ouvre le ticket “VPN down (remote)”.
3. Vérifie :
   - Le titre et la description
   - Le statut (souvent “À faire/Open” au début)
   - La section SLA (si configurée)
   - Les détails (priorité, assigné…)

✅ Résultat attendu
- Le ticket s’affiche correctement (ex : SUP-2 (ou autre))
- La vue agent montre les champs utiles

📸 Preuve (capture)
![](../screenshots/02-incident-vpn-ticket.png)

---

## 5) (Important) Vérifier les rôles / accès dans le projet Support

### Pourquoi c’est important
Si tu n’as pas les bons rôles, tu vas voir des erreurs du type :
- “Impossible d’afficher ce projet”
- “Vous n’êtes pas autorisé”
- Impossible de configurer workflows/SLA/automation

### Comment le faire (détaillé)
1. Dans le projet **Support**, va dans **Paramètres de l’espace**.
2. Menu gauche → **Accès** → **Personnes et accès**.
3. Sur ton compte, vérifie que tu as au minimum :
   - ✅ **Administrators**
   - ✅ **Service Desk Team**

✅ Résultat attendu
- Tu es admin + agent (service desk team)
- Tu peux configurer l’espace

---

## 6) Workflow : vérifier le flux d’états du ticket

### Ce qu’on fait
On regarde le workflow (les états possibles d’un ticket) pour comprendre comment il circule.

### Comment le faire (détaillé)
1. Projet Support → **Paramètres de l’espace**
2. Menu gauche → **Gestion des demandes** → **Workflows**
3. Ouvre le workflow et affiche “diagramme”.

✅ Résultat attendu
Tu dois voir un workflow simple avec des états du type :
- OPEN
- PENDING
- WORK IN PROGRESS
- DONE
- REOPENED

📸 Preuve (capture)
![](../screenshots/03-workflow.png)

---

## 7) SLA : vérifier les objectifs de délai

### Ce qu’on fait
On vérifie que des SLA existent (même basiques), et qu’ils sont applicables aux tickets.

### Comment le faire (détaillé)
1. Projet Support → **Paramètres de l’espace**
2. Menu gauche → **Gestion des demandes** → **SLA**
3. Tu dois voir des objectifs comme :
   - **Time to first response**
   - **Time to resolution**

✅ Résultat attendu
- Les SLA existent et sont listés
- Ils pourront apparaître sur les tickets

📸 Preuve (capture)
![](../screenshots/04-sla.png)

---

## 8) Automation : auto-assign si le titre contient “VPN”

### Ce qu’on fait
On crée une règle simple :
> Si un ticket est créé et que le résumé contient “VPN” → assigner à moi.

### Logique de la règle (simple)
- **When** : Ticket créé
- **If** : Projet = Support (SUP)
- **If** : Résumé contient “VPN”
- **Then** : Assigner le ticket à (toi)

### Comment le faire (détaillé)
1. Va dans **Automation** (selon ton interface : projet ou admin Jira).
2. Crée une nouvelle règle.
3. Ajoute le déclencheur **Ticket créé**.
4. Ajoute les conditions :
   - Projet = Support (SUP)
   - Résumé contient `VPN`
5. Ajoute l’action :
   - Assigner le ticket à (toi)
6. **Active la règle**.

✅ Résultat attendu
- La règle est ACTIVE
- Les prochains tickets avec “VPN” seront assignés automatiquement

📸 Preuve (capture)
![](../screenshots/05-automation.png)

### Problème fréquent (et solution)
**Symptôme :** tu cliques sur “règle sans titre” / tu arrives sur un écran vide / “exemple de règle”  
**Solution :** repars sur une création de règle “propre” et vérifie bien que tu ajoutes :
- un déclencheur
- au moins une condition
- une action
puis active la règle.

---

## 9) Dashboard : créer un “SUP - ITSM Overview”

### Ce qu’on fait
On crée un tableau de bord avec 3 gadgets simples :
- un graphe par état
- une liste de tickets (résultats de filtre)
- une matrice “assigné x état”

### Comment le faire (détaillé)
1. Va dans **Tableaux de bord**.
2. Clique **Créer un tableau de bord**
3. Nom : **SUP - ITSM Overview (Portfolio)**
4. Laisse les accès en **Privé** pour l’instant.
5. Enregistre.

✅ Résultat attendu
- Un dashboard créé (au début il est vide)

📸 Preuve (capture)
![](../screenshots/06-dashboard.png)

### Gadgets ajoutés (ceux qu’on a utilisés)

#### A) “Graphique à secteurs” (par État)
- Filtre : `Filter for SUP`
- Type de statistique : **État**
- Enregistrer

#### B) “Résultats du filtre”
- Filtre : `Filter for SUP`
- Colonnes : inclure **État** et **Personne assignée**
- Enregistrer

#### C) “Statistiques de filtre bidimensionnel”
- Filtre : `Filter for SUP`
- Axe X : **Personne assignée**
- Axe Y : **État**
- Enregistrer

### Problème fréquent : erreur 403 dashboard / “Impossible d’afficher”
**Symptôme :** “Impossible d’afficher ce tableau de bord” (403)  
**Solution :**
- revenir via “Parcourir les tableaux de bord”
- ouvrir celui dont tu es propriétaire
- vérifier que tu es bien connecté au bon compte / bon site Atlassian

---

## 10) Résultat final (checklist)

- [x] Portail Help Center fonctionne
- [x] Ticket VPN créé via portail
- [x] Workflow visible en diagramme
- [x] SLA visibles (time to first response / resolution)
- [x] Automation active (assign si “VPN”)
- [x] Dashboard créé + gadgets ajoutés
