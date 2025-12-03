# JSM — Créer une *file d’attente* (Queue) “All open” (pas à pas)

> Objectif : avoir une vue “agents” simple qui liste **tous les tickets ouverts / non résolus**, avec les colonnes utiles (assigné, état, dates, SLA), pour piloter le support au quotidien.

---

## 1) Où ça se passe ?

Dans ton projet JSM (ex: **Support**) :

- **Files d’attente (Queues)** → **Créer / Nouvelle file d’attente**

Tu arrives sur l’écran “**Nouvelle file d’attente**” (celui de ta capture).

---

## 2) Comprendre l’écran “Nouvelle file d’attente”

Tu as 3 blocs importants :

### A) *Assigner au groupe de priorités* (optionnel)
- Ça sert si tu utilises des **Groupes de priorités** (ex: L1 / L2 / VIP / etc.) pour organiser le travail des agents.
- Si tu n’en utilises pas : laisse vide pour l’instant (c’est OK).

### B) *Filtrer par* (le cœur de la queue)
Tu construis le filtre qui détermine **ce qui apparaît** dans la file.

Sur ta capture, tu es déjà sur :
- **Résolution : Non résolu** ✅  
…donc tu listes tous les tickets encore ouverts (non “résolus/terminés”).

Tu peux faire ça en mode :
- **De base** : filtres “cliquables” (simple)
- **JQL** : version “power user” (plus précis)

### C) *Colonnes*
Tu choisis quelles infos s’affichent dans la liste (ce que voient les agents).

Sur ta capture, tu as déjà :
- Type de ticket
- Clé
- Résumé
- Rapporteur
- Personne assignée
- État
- Création

---

## 3) Recette “All open” (simple & propre)

### Étape 1 — Garde le filtre “non résolu”
En **De base** :
- **Résolution → Non résolu**

✅ Résultat : ta queue affiche *tout ce qui n’est pas terminé*.

### Étape 2 — (Optionnel) Filtrer seulement certains types de demande
Si tu veux que “All open” ne montre que les tickets du support IT (tes formulaires), tu peux ajouter :

En **De base** :
- **Type de demande** → sélectionne :
  - Incident - VPN (Remote)
  - Request - Access
  - Request - Hardware
  - Request - Onboarding
  - (Ask a question si tu veux)

Sinon, laisse “All open” volontairement large (ça peut être utile au début).

### Étape 3 — (Optionnel) Filtrer par états “actifs”
Ça dépend de ton workflow, mais souvent on exclut les états “Done/Terminé”.
Comme tu filtres déjà en “Non résolu”, c’est souvent suffisant.

---

## 4) Colonnes recommandées (pour un support réaliste)

Garde tes colonnes actuelles + ajoute (si dispo chez toi) :

- **Priorité** (très utile)
- **SLA : Time to first response**
- **SLA : Time to resolution**

Pourquoi ?
- Ça te permet de voir rapidement ce qui approche du dépassement SLA.
- Et tu peux trier la queue sur le SLA (comme tu as déjà fait avec tes vues).

> Si tu ne vois pas les colonnes SLA : c’est généralement parce que le SLA n’est pas encore appliqué au type de ticket, ou que la queue ne montre pas des tickets concernés par ce SLA.

---

## 5) Enregistrer & valider

1. Donne un nom à la queue (ex: **All open**).
2. Clique **Enregistrer**.
3. Vérifie :
   - tu vois les tickets attendus (ex: SUP-1, SUP-2…)
   - les colonnes affichent bien “Assignée / État”
   - si SLA est activé : tu peux trier/voir les métriques

---

## 6) Dépannage (les galères fréquentes)

### “Je ne vois pas mes tickets dans la file”
- Vérifie **Résolution : Non résolu** (si le ticket est déjà “Terminé”, il sort)
- Vérifie si tu as ajouté un filtre trop restrictif (ex: type de demande)

### “Je ne vois pas les champs SLA”
- Vérifie que tu as bien **créé un SLA** dans le projet
- Vérifie que le SLA “s’applique” à tes tickets (champ “Appliquer aux tickets” / JQL)
- Vérifie que tu affiches des tickets concernés (ex: Incident VPN)

---

## 7) Bonus (version JQL “pro”)

Si tu veux un équivalent JQL de base (à adapter) :

```jql
project = SUP AND resolution = Unresolved ORDER BY created DESC
```

Et si tu veux ne voir que tes 4 formulaires (adapter aux noms exacts) :

```jql
project = SUP
AND resolution = Unresolved
AND "Request Type" IN ("Incident - VPN (Remote)", "Request - Access", "Request - Hardware", "Request - Onboarding")
ORDER BY created DESC
```

---

### Prochaine étape logique
Créer 2–3 queues supplémentaires, typiques d’un vrai support :
- **Unassigned** (non assignés)
- **My tickets** (assignés à moi)
- **Breached / At risk** (trié par SLA, si tu veux simuler la pression support)
