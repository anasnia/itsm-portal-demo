# Solution Design — ITSM Portal (Jira Service Management + Confluence)

## 1) Contexte (simple)
Aujourd’hui certaines demandes IT arrivent par mail/Teams → pas de suivi clair, pas de règles, pas de délais.
Objectif : mettre en place un portail unique avec des tickets, des règles d’assignation, des SLA et des KPI.

## 2) Périmètre (V1)
### Use cases
- **Incident** : VPN ne fonctionne pas
- **Demande** : accès à une application
- **Demande** : nouveau matériel
- **Demande** : onboarding (arrivée d’un nouveau)

### Objectifs mesurables
- 100% des demandes passent par le portail (plus de mails “dans le vide”)
- SLA configurés et suivis
- Assignation automatique par type de demande
- Dashboard avec KPIs de base

## 3) Catalogue / Request types (ce que l’utilisateur voit)
### A) Incident — VPN
Champs minimum :
- Impact (1-3), Urgence (1-3)
- OS (Windows/Mac), Localisation (Bureau/Remote)
- Description

### B) Demande — Accès application
Champs minimum :
- Application (liste)
- Type d’accès (Read/Write/Admin)
- Manager (approbateur)
- Date souhaitée

### C) Demande — Nouveau matériel
Champs minimum :
- Type (Laptop/Écran/Clavier/etc.)
- Justification
- Manager (approbateur)
- Site

### D) Demande — Onboarding
Champs minimum :
- Date d’arrivée
- Manager (approbateur)
- Équipe
- Matériel nécessaire (multi-select)
- Accès nécessaires (multi-select)

## 4) Process / Workflow (ce que fait l’IT)
Workflow unique (6 statuts) :
1. **New**
2. **Triage**
3. **Waiting for approval**
4. **In progress**
5. **Waiting for customer**
6. **Done**

Règles simples :
- **Waiting for approval** seulement pour : Accès / Matériel / Onboarding
- **Pause SLA** en Waiting for approval + Waiting for customer
- Passage en **Done** uniquement si un champ “Resolution” est rempli

## 5) Priorité (comprendre P1/P2…)
La priorité est calculée à partir de **Impact × Urgence**.

Mapping simple (suffisant pour une démo) :
- **P1** : Impact=1 et Urgence=1
- **P2** : (1,2) ou (2,1)
- **P3** : (2,2)
- **P4** : le reste

Exemples :
- VPN down pour beaucoup de personnes → P1/P2
- Matériel demandé “pour confort” → P4

## 6) SLA (les délais)
Deux SLA :
- **Time to first response**
- **Time to resolution**

Cibles (exemple) :
First response:
- P1 30 min, P2 2h, P3 8h, P4 24h

Resolution:
- P1 4h, P2 8h, P3 3 jours, P4 5 jours

Pause SLA si statut =
- Waiting for approval
- Waiting for customer

## 7) Automations (les règles qui font gagner du temps)
Minimum set :
1) **Auto-assign** par type de demande :
- Incident VPN → groupe “Support N1”
- Accès application → groupe “IT Apps”
- Matériel / Onboarding → groupe “Workplace”

2) **Auto-priorité** à partir de Impact/Urgence

3) **Alerte SLA** (avant breach) :
- commentaire interne + notification équipe

4) **Auto-close** :
- si Waiting for customer > 3 jours sans réponse → fermer avec commentaire

5) **Onboarding = sous-tâches**
À l’entrée “In progress”, créer 3 sous-tâches :
- Créer comptes
- Préparer matériel
- Donner accès applications

## 8) Knowledge Base (Confluence)
Créer au moins 1 article KB :
- "VPN — diagnostic rapide"
Objectif : réduire les tickets répétitifs + accélérer la résolution.

## 9) KPI / Dashboard (pilotage)
KPI minimum :
- Volume de tickets par semaine
- Backlog par équipe (assigned group)
- Respect SLA (% breached vs on track)
- Temps moyen de résolution par type de demande

## 10) Preuves attendues (portfolio)
- Captures : portail, workflow, SLA, automation, dashboard, KB
- Runbook admin : conventions + gouvernance + “qui modifie quoi”
