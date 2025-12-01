# ITSM pour débutants (niveau “je pars de zéro / intorduction au "vocabulaire"”)

## C’est quoi l’ITSM ?
ITSM veut dire **IT Service Management** : c’est la façon “organisée” dont une entreprise gère l’aide informatique.
Comme un **service client**, mais pour l’informatique.

But : que les problèmes soient traités **vite**, **dans le bon ordre**, et qu’on puisse **mesurer** la qualité.

---

## Pourquoi on utilise des tickets ?
Sans tickets :
- les gens envoient des mails partout
- on oublie des demandes
- impossible de savoir ce qui est urgent
- aucune statistique

Avec des tickets :
- chaque demande a un numéro
- on sait qui fait quoi
- on suit des délais
- on peut faire des stats et s’améliorer

---

## Les 3 choses à connaître (super important)

### 1) Incident = “c’est cassé”
Quelque chose marchait et ne marche plus.

Exemples :
- “Je n’ai plus Internet”
- “Le VPN ne marche pas”
- “Je ne peux pas ouvrir ma boîte mail”

Objectif : **réparer** et rétablir le service.

---

### 2) Demande (Request) = “je veux quelque chose”
Rien n’est cassé, mais tu veux un service.

Exemples :
- “J’ai besoin d’un accès à une application”
- “Je veux un nouvel écran”
- “Créer un compte pour un nouvel arrivant”

Objectif : **livrer** ce qui est demandé.

---

### 3) Change = “on modifie un système”
On change quelque chose en prod, donc on planifie et on contrôle.

Exemples :
- mise à jour d’un serveur
- changement de configuration réseau
- déploiement d’une nouvelle version

Objectif : **changer sans casser**.

---

## Priorité : c’est quoi P1/P2/P3/P4 ?
La priorité sert à décider “qui passe en premier”.

### Impact = combien de personnes sont touchées ?
- 1 personne : impact faible
- toute une équipe : impact moyen
- toute la boîte : impact fort

### Urgence = est-ce que c’est bloquant maintenant ?
- ça peut attendre : urgence faible
- ça bloque le travail : urgence forte

Exemples :
- VPN down pour tout le monde → impact fort + urgence forte → **P1**
- clavier cassé pour 1 personne → impact faible + urgence faible → **P4**

---

## SLA : le chrono
Un SLA, c’est un engagement de temps, par exemple :
- “On répond à un P1 en moins de 30 minutes”
- “On résout un P2 en moins de 8 heures”

En général on mesure :
- **temps de première réponse**
- **temps de résolution**

---

## Escalade : quand on passe à plus expert
Si le support N1 n’arrive pas à résoudre :
- escalade vers N2 (plus technique)
- puis N3 (expert)

Exemple :
- N1 voit que le problème vient du réseau → escalade vers l’équipe réseau.

---

## KB (Knowledge Base) : les tutos officiels
Une KB est une bibliothèque d’articles pour résoudre vite.

Exemples :
- “VPN : diagnostic rapide”
- “Réinitialiser son mot de passe”
- “Comment demander un accès à l’application X”

---

## Mini histoire (pour tout relier)
1) Tu n’as plus accès au VPN → **Incident**
2) Tu demandes un accès à Jira → **Demande**
3) L’IT planifie une mise à jour dimanche → **Change**
4) Si le VPN bloque tout le monde → ticket **P1**
5) Le SLA impose : réponse < 30 min
6) Si N1 bloque → **escalade** vers N2
7) La KB aide à résoudre plus vite

---

## Résumé en 6 lignes
- Incident = cassé
- Demande = je veux quelque chose
- Change = on modifie un système
- Priorité = impact × urgence
- SLA = chrono
- KB = tutos
