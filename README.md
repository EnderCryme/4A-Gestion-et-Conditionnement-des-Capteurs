# A8_4PSM — Gestion et Conditionnement des Capteurs
**Réalisé par V. Cauquil, A. Falada, T. Ghayad, K. Sabra**

Ce dépôt regroupe l’intégralité du projet, de la simulation à la documentation, autour de la conception d’un système de gestion et de conditionnement pour capteurs (résistifs, capacitifs et jauges de contrainte), via des régulateurs, amplificateurs et circuits dédiés. La chaîne de conception — du PCB sous KiCad à la validation par simulation LTspice — s’accompagne d’un process salle blanche assurant précision et fiabilité.

## Gestion des Capteurs & Process de Salle Blanche

Le projet se concentre sur la conception et l’implémentation d’un système de gestion et de conditionnement des capteurs capable de traiter plusieurs types de signaux (résistifs, capacitifs, jauges de contrainte), pour des applications exigeantes en précision.  
Parmi les circuits réalisés :
- régulateurs de tension (conversion 5 V vers 3,3 V),
- générateurs de courant,
- amplificateurs d’instrumentation,
- circuits de linéarisation et de soustraction.

La conception du schéma et du routage PCB a été effectuée sous **KiCad**, avec une validation et pré-qualification des montages assurée par la simulation **LTspice**.  
Une attention particulière a été portée aux process salle blanche, assurant des conditions optimales lors de l’assemblage et l’intégration de composants sensibles, dans le but de garantir fiabilité et reproductibilité pour les dispositifs de haute précision.

---

## Contenu du dépôt

Vous trouverez dans ce dépôt :
- Tous les **fichiers de simulation LTspice** (prévalidation des montages avant passage sous KiCad),
- Les **fichiers de conception PCB sous KiCad**,
- Les **codes de programmation STM32** pour la gestion des capteurs,
- Les documents associés au projet :
  - Le **rapport de conception** : `A8_Capteurs_GHAYAD_CAUQUIL_FALDA_SABRA.pdf`
  - Le **poster de présentation** : `A8_Posteur_CAUQUIL_SABRA_GHAYAD_FALDA.pdf`

### Détail des simulations LTspice :
- **Resistive.asc** : 
  Correspond au montage résistif, chaque étage étant séparé :
  1. Convertisseur Tension-Courant
  2. Amplificateur d'instrumentation
  3. Deux montages de soustraction (pour comparer deux solutions)
- **Capacitif.asc** : 
  Inclut les montages NE555 en configuration monostable et astable.

