# Runbook Admin — Jira Service Management (ITSM Portal)

## 1) Objectif
Ce document décrit les règles d’administration et de gouvernance :
- qui modifie quoi
- comment on teste
- conventions de nommage
- rituels d’exploitation (SLA/KPI/KB)

---

## 2) Rôles & responsabilités (RACI simplifié)
- **JSM Admin** : gère config (workflows, champs, permissions, automations), valide les changements.
- **Service Desk Lead** : définit le process (SLA, priorités, catégorisation), arbitre.
- **Agents (N1/N2)** : traitent les tickets, enrichissent la KB, remontent les besoins.
- **Request Approvers (Managers)** : approuvent les demandes (accès, matériel, onboarding).

---

## 3) Gouvernance des changements (configuration)
### Règles
- Pas de changement “en prod” sans test.
- Toute modification doit être traçable (qui / quoi / pourquoi).

### Process (simple)
1) Proposer un changement (ticket “Config change” ou issue interne)
2) Implémenter sur projet de test / sandbox (si dispo)
3) Valider (Admin + Lead)
4) Déployer en prod
5) Documenter (release note courte)

### Log de changements (modèle)
- Date
- Élément modifié (workflow/SLA/champ/automation)
- Motif
- Auteur
- Risque / rollback

---

## 4) Conventions de nommage
### Request types
- `Incident - VPN`
- `Request - Access`
- `Request - Hardware`
- `Request - Onboarding`

### Champs
- noms courts, explicites, sans doublons
- ex : `Impact`, `Urgency`, `Application`, `Access type`, `Manager`

### Catégories (baseline)
- Réseau
- Poste de travail
- Applications
- Sécurité
- Matériel

---

## 5) Standards d’exploitation (run)
### Triage
- Triage quotidien (ex: 2 fois/jour)
- Vérifier : priorité, catégorie, assignation, info manquante

### SLA
- Revue hebdo : tickets breached + causes + actions
- Revue mensuelle : ajustement des cibles si besoin

### Knowledge Base (KB)
- Tout incident récurrent doit donner lieu à un article KB
- Revue mensuelle : pages obsolètes / mises à jour

---

## 6) Sécurité & bonnes pratiques
- Ne pas exposer données sensibles dans les champs/KB
- Masquer/anonymiser les screenshots du portfolio
- Aucun token / secret stocké dans GitHub
