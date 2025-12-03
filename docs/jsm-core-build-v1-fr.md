# JSM Core Build (v1) — Mise en place de base (pas à pas) + pourquoi

Ce document décrit la mise en place “core” réalisée sur **Jira Service Management (Cloud)** dans le projet **Support (SUP)**.
L’idée : apprendre en pratiquant et garder une trace claire, reproductible, et compréhensible même pour un débutant.

---

## 0) Périmètre (ce que couvre ce guide)
- Portail (Help Center) + structuration des types de demandes
- Création de tickets réels via le portail (tests)
- Vérification du workflow (diagramme)
- Mise en place des SLA (première réponse + résolution)
- Mise en place d’une automatisation (auto-assign VPN)
- Vérifications et tests (pour s’assurer que tout fonctionne)

---

## 1) Portail : structurer les “types de demande” (Request types)

### Ce que j’ai fait
J’ai organisé le portail “IT Support” avec des types de demandes **clairs** :
- `Incident - VPN (Remote)`
- `Request - Access`
- `Request - Hardware`
- `Request - Onboarding`
- `Ask a question`

Et j’ai masqué les types de demandes génériques/inutiles sur le portail :
- `Emailed request` (masqué)
- `Submit a request or incident` (masqué) pour éviter le “fourre-tout”.

### Pourquoi (objectif)
Des types de demande bien définis :
- réduisent les échanges inutiles (moins de “tu peux préciser ?”)
- standardisent les informations collectées
- facilitent le tri (routing) côté agents
- améliorent l’expérience utilisateur côté portail

### Comment (pas à pas)
1. Ouvrir le projet : **Support (SUP)**
2. Aller dans **Paramètres de l’espace** → **Types de demande**
3. Créer / renommer les types de demandes listés plus haut
4. Mettre tous ces types de demande dans le même **Groupe de portails** : `IT Support`
5. Masquer sur le portail les types non souhaités :
   - `Emailed request` → **Masqué sur le portail**
   - `Submit a request or incident` → **Masqué sur le portail**
6. Aller dans **Paramètres de l’espace** → **Portal** → **Groupes de portail**
7. Ouvrir le groupe `IT Support` et **réordonner** les demandes (drag & drop) pour afficher d’abord les demandes les plus importantes
8. Valider le rendu via **Afficher le portail** (Help Center)

### Preuves (captures)
- Entrée Help Center / portail :
  - `screenshots/Portail_itsm.png`

---

## 2) Créer des tickets réels via le portail (tests)

### Ce que j’ai fait
J’ai créé des tickets depuis le portail (ex: incident VPN) pour avoir des tickets “réels” et tester la configuration.

### Pourquoi (objectif)
Tester “en conditions réelles” permet de vérifier :
- que le portail fonctionne
- que les types de demande créent bien des tickets côté agent
- que l’automation se déclenche sur un cas réel
- que les SLA s’affichent et comptent correctement

### Comment (pas à pas)
1. Aller sur le portail (Help Center)
2. Cliquer sur un type de demande, ex : `Incident - VPN (Remote)`
3. Saisir un résumé clair, ex :
   - `[VPN][Remote] Authentication failed - user blocked`
4. Envoyer la demande

### Preuves (captures)
- Écran de création d’une demande :
  - `screenshots/create_incident_request.png`
- Exemple de ticket créé côté agent (VPN) :
  - `screenshots/02-incident-vpn-ticket.png`

---

## 3) Workflow : vérifier le cycle de vie du ticket

### Ce que j’ai fait
J’ai consulté le workflow (diagramme) utilisé par le projet.

### Pourquoi (objectif)
Le workflow est la base :
- il définit les statuts possibles (Open, Pending, Done, etc.)
- il conditionne les SLA (pause/stop), les automatisations et le reporting

### Comment (pas à pas)
1. Projet Support → **Paramètres de l’espace**
2. Aller dans **Gestion des demandes** → **Workflows**
3. Ouvrir le workflow et afficher le diagramme

### Preuves (captures)
- Diagramme workflow :
  - `screenshots/03-workflow.png`

---

## 4) Automation : auto-assign des incidents VPN

### Ce que j’ai fait
J’ai créé une règle d’automatisation qui assigne automatiquement les tickets VPN.

### Pourquoi (objectif)
Automatiser l’assignation :
- réduit le temps de tri manuel (triage)
- accélère la prise en charge
- standardise le routage des tickets

### Règle (logique)
- **Déclencheur** : ticket créé
- **Conditions** :
  - projet = SUP
  - résumé contient "VPN"
- **Action** : assigner le ticket à moi

### Comment (pas à pas)
1. Ouvrir **Automation**
2. Créer une nouvelle règle
3. Ajouter :
   - Déclencheur : **Ticket créé**
   - Condition : **Projet = SUP**
   - Condition : **Résumé contient "VPN"**
   - Action : **Assigner le ticket** (à moi)
4. Activer la règle
5. Valider en créant un **nouveau** ticket VPN via le portail (l’assignment doit se faire automatiquement)

### Preuves (captures)
- Règle d’automation active :
  - `screenshots/05-automation.png`

---

## 5) SLA : première réponse + résolution

### Ce que j’ai fait
J’ai créé deux SLA :
1. **Time to first response (8h)** : mesurer le délai avant la première réponse
2. **Time to resolution** : mesurer le délai total avant clôture

### Pourquoi (objectif)
Les SLA permettent :
- de piloter la performance (délais de réponse/résolution)
- de prioriser
- d’alimenter les dashboards et les routines d’exploitation

---

### 5.1 SLA #1 — Time to first response (8h)

#### Objectif
- 8 heures

#### Conditions (logique)
- **Démarrer** : ticket créé
- **Pause** : statut = Pending
- **Arrêter** : condition de commentaire (configurée dans JSM)

#### Comment (pas à pas)
1. Projet Support → **Paramètres de l’espace** → **SLA**
2. Cliquer **Ajoutez un SLA**
3. Nommer : `Time to first response`
4. Mettre l’objectif : `8h`
5. Définir les conditions (démarrer / pause / arrêter)
6. Enregistrer
7. Ouvrir un ticket et vérifier que le bloc **SLA** apparaît à droite

---

### 5.2 SLA #2 — Time to resolution

#### Objectif
- (exemple) durée de résolution sur la totalité du cycle de vie

#### Conditions (logique)
- **Démarrer** : ticket créé
- **Pause** : statut = Pending
- **Arrêter** : statut = `Terminé(e)` (Done)

#### Comment (pas à pas)
1. Projet Support → **Paramètres de l’espace** → **SLA**
2. Cliquer **Ajoutez un SLA**
3. Nommer : `Time to resolution`
4. Définir :
   - Start : Ticket créé
   - Pause : Pending
   - Stop : Terminé(e)
5. Enregistrer
6. Tester sur un ticket :
   - passer en Pending → le SLA doit se mettre en pause
   - passer en Terminé(e) → le SLA doit s’arrêter

### Preuves (captures)
- Écran de configuration SLA :
  - `screenshots/04-sla.png`

---

## 6) Vérifications (checklist de validation)

Après la configuration, j’ai vérifié :
- [x] Le portail affiche uniquement les demandes utiles dans `IT Support`
- [x] Un ticket VPN créé via portail génère bien un ticket SUP côté agent
- [x] L’automation assigne automatiquement les nouveaux tickets VPN
- [x] Le bloc SLA est visible sur le ticket
- [x] Le SLA “pause” en Pending et “stop” en Terminé(e)

---

## 7) Notes pratiques (problèmes courants + solutions)

### 7.1 SLA invisible sur les tickets
**Cause fréquente :** aucun SLA n’a été créé dans le projet.  
**Solution :** créer au moins 1 SLA + vérifier les conditions Start/Stop.

### 7.2 Un ticket ancien n’a pas été assigné par l’automation
**Cause :** l’automation ne s’applique pas rétroactivement.  
**Solution :** tester avec un nouveau ticket créé après activation.

### 7.3 L’ordre des tuiles du portail ne change pas depuis “Types de demande”
**Cause :** l’ordre se gère dans **Portal → Groupes de portail** (drag & drop).  
**Solution :** réordonner dans le groupe `IT Support` et actualiser le portail.

---
