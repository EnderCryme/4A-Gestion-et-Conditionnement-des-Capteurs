# Dossier Technique — STM32 : Mesure de Capteurs Résistif, Capacitif et Jauge de Contrainte

## Objectif

Ce projet permet de mesurer différents types de capteurs analogiques via l’ADC intégré d’un microcontrôleur STM32, avec interaction par communication série (UART).

---

## Schéma des Entrées / Sorties Utilisées

| Broche STM32 | Fonction                           | Description                                   |
|--------------|------------------------------------|-----------------------------------------------|
| **PA6**      | `ADC_CHANNEL_6`                    | Lecture d’un montage **capacitif**            |
| **PA7**      | `ADC_CHANNEL_7`                    | Lecture d’un montage **résistif**             |
| **PB0**      | `ADC_CHANNEL_8` *(non utilisé ici)*| Lecture d’une **jauge de contrainte**         |
| **PA3**      | `TIM2_CH4 (PWM)`                   | Alimentation **PWM** pour le montage capacitif |
| **PC4/PC5**  | `LPUART1_TX / RX`                  | Communication UART (console série)            |

> **Note** : Pour le montage capacitif, connecter physiquement **PA3** (PWM) au circuit du capteur via un fil.

---

## ⚙️ Paramètres de la Communication UART

| Paramètre     | Valeur             |
|---------------|--------------------|
| Vitesse (baudrate) | `19200`       |
| Bits de données    | `8`           |
| Parité             | `Aucune`      |
| Bits d'arrêt       | `1`           |
| Pins utilisées     | `LPUART1_TX (PC4)` / `LPUART1_RX (PC5)` |

---

## ⏱️ Configuration du Timer (PWM sur PA3)

Le timer TIM2 est configuré pour générer un signal PWM sur le **canal 4 (PA3)** :

| Paramètre       | Valeur                    |
|-----------------|---------------------------|
| Périphérique    | `TIM2`                    |
| Canal           | `CH4`                     |
| Fréquence       |  3.3 KHz|
| Duty Cycle      | ~90% (Pulse = 572 / Period = 635) |
| Mode            | `PWM1`                    |

---

## Commandes UART disponibles

Une fois le programme lancé, vous pouvez envoyer une commande via le terminal série (ex : TeraTerm, PuTTY, etc.) pour interagir :

| Commande | Action réalisée                                        |
|----------|--------------------------------------------------------|
| `a`      | Lit **tous les canaux** disponibles (PA6 et PA7)       |
| `6`      | Lit uniquement **PA6** (capteur capacitif)             |
| `7`      | Lit uniquement **PA7** (capteur résistif)              |
| `8`      | ⚠️ Non opérationnel, affiche une erreur                |
| Autre    | Affiche les commandes disponibles                      |

---

## Exemple de Résultat (UART)

```
> a
Mesure de PA6 (CH6) en cours...
PA6 (CH6): 1234 (0.995 V)
Mesure de PA7 (CH7) en cours...
PA7 (CH7): 3098 (2.495 V)

> 7
Mesure de PA7 (CH7) en cours...
PA7 (CH7): 2987 (2.405 V)
```
❗ **Attention** : Lorsqu’un **seul capteur est connecté et lu**, les mesures ADC sont **cohérentes et fiables**.  
En revanche, lorsque **plusieurs capteurs sont branchés et lus simultanément via le même ADC**, la lecture retourne systématiquement **la tension de référence Vref (≈3.3V)**. Cela peut être dû à un **manque de temps de stabilisation**, un **conflit de chemins analogiques**, ou une **configuration incomplète du multiplexer ADC**. 
> Malheureusement, nous n'avons pas pu par manque de temps adressé cette problématique.

---

## Configuration du projet (CubeMX / STM32)

- **ADC** : Résolution 12 bits, mode single-ended, calibration activée
- **UART (LPUART1)** : Communication asynchrone
- **TIMER2 (CH4)** : PWM activé pour générer un signal d’alimentation
- **Interruption UART** : Utilisée pour traiter les commandes utilisateurs en temps réel

---

## Notes supplémentaires

- Le canal **PB0 (ADC_CHANNEL_8)** est référencé dans le code mais n’est **pas implémenté** pour la lecture : il génère une erreur volontaire.
- La calibration ADC est automatique au démarrage.
- Le code actuel suppose une lecture analogique directe via l’ADC.  
  --> Il **serait pertinent d’ajouter un mode de configuration** :
  - **Astable** : utiliser un timer (en capture d’impulsions) pour mesurer la **fréquence d’oscillation** du montage.
  - **Monostable** : maintenir l’usage de l’**ADC** pour lire une **valeur moyenne de tension**.
- Cette fonctionnalité pourrait être activée via une **commande UART supplémentaire** (par exemple `m` pour monostable et `s` pour astable), et permettrait de s’adapter dynamiquement au type de capteur connecté.

---
