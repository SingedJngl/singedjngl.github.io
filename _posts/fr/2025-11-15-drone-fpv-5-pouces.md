---
layout: post
title: "Monter et régler un FPV 5 pouces en analogique"
ref: fpv-5in
lang: fr
permalink: /journal/drone-fpv-5-pouces/
cover: /assets/img/build_fpv_final.jpg
cover_alt: "Le drone FPV 5 pouces terminé"
cover_caption: "Le drone FPV 5 pouces terminé"
excerpt_text: "Conception, assemblage et configuration d'un drone FPV freestyle 5 pouces sous Betaflight."
stack: ["Betaflight", "ESC 4-en-1", "Vidéo analogique"]
---

Ce drone est un cinq pouces de freestyle en "bando", monté pièce par pièce, en
transmission vidéo analogique. J'aurais pu partir sur du numérique, mais l'analogique
reste moins cher, plus tolérant quand le signal se dégrade, et suffisant pour
apprendre.

## Vue d'ensemble
Conception, assemblage et configuration d'un drone FPV freestyle 5 pouces sous Betaflight.

## Objectifs
- Construire un drone FPV fiable et facile à entretenir
- Optimiser la stabilité en vol
- Configurer le contrôleur de vol, l'ESC, le récepteur radio et le VTX
- Comprendre le réglage des PID et du filtrage

## Matériel utilisé
| Composant | Référence |
|-----------|-----------|
| Châssis | MotorRiot Tanq2 |
| Contrôleur de vol | Mamba MK4 H743 V2 |
| ESC | Diatone 4-en-1 F55 128K |
| Moteurs | Velox V2207 V2 1750KV |
| Caméra FPV | Foxeer T-Rex mini |
| Émetteur vidéo (VTX) | SpeedyBee TX800 |
| Récepteur radio | RadioMaster Nano ELRS RP1 2,4 GHz V2 |
| Batterie | LiPo Tattu 6S 1300 mAh |
| Buzzer | Vifly Finder 2 (buzzer autonome) |

## Architecture du système
Le drone est construit autour d'un contrôleur de vol H7 relié à un ESC 4-en-1, quatre moteurs brushless, un récepteur ExpressLRS, une caméra FPV et un émetteur vidéo analogique.

Le schéma de câblage ci-dessous résume les principales liaisons électriques et de signal.
<img src="{{ '/assets/img/diatone-mamba-h7-fc-flight-controller-manual-instructions-wiring.webp' | relative_url }}" alt="Schéma de câblage du drone FPV" width="700">

Liaisons principales :
- La batterie LiPo alimente directement l'ESC 4-en-1.
- L'ESC alimente le contrôleur de vol et communique avec lui.
- Les moteurs sont pilotés par l'ESC via le protocole DShot.
- Le récepteur ExpressLRS communique avec le contrôleur de vol en CRSF sur UART.
- La caméra FPV est reliée au contrôleur de vol pour l'incrustation de l'OSD.
- Le VTX reçoit la sortie vidéo du contrôleur de vol et se configure en IRC Tramp sur UART.

## Configuration logicielle
- Firmware : Betaflight
- Système vidéo : analogique, émetteur piloté en IRC Tramp
- Protocole ESC : DShot 600
- Protocole radio : CRSF

Côté ports série, seulement deux UART sont réellement utilisés : l'UART1 en *Serial RX*
pour le récepteur ExpressLRS, et l'UART3 en périphérique *VTX (IRC Tramp)* pour piloter
le canal et la puissance d'émission directement depuis l'OSD. Le reste est laissé
désactivé, ça évite les conflits au moment du boot.
<img src="{{ '/assets/img/bf-uart.webp' | relative_url }}" alt="Onglet Ports de Betaflight avec UART1 en Serial RX et UART3 en VTX IRC Tramp" width="700">

## Réglages

### Failsafe
C'est la première chose que j'ai configurée, avant même de faire tourner les moteurs.
L'étape 1 remet les voies roll/pitch/yaw/throttle sur *Auto* dès que le signal devient
invalide, et les AUX restent en *Hold*. Si la perte dure plus de 1,5 s, l'étape 2
déclenche la procédure *Drop* : le drone coupe les moteurs et tombe sur place. Sur un
terrain de freestyle c'est plus sûr qu'un retour maison approximatif sans GPS.
<img src="{{ '/assets/img/bf-failsafe.webp' | relative_url }}" alt="Onglet Failsafe de Betaflight, étape 1 et étape 2 configurées" width="700">

### PID
Je suis parti des valeurs par défaut et j'ai travaillé essentiellement avec les sliders
plutôt qu'en touchant chaque terme à la main. Le *Master Multiplier* est monté à 1.50
pour compenser l'inertie de la machine, qui reste lourde avec ses 720 g. On retrouve
ensuite en bas les valeurs effectives : 67/120/49 en roll, 70/126/56 en pitch, et un
D à 0 en yaw comme il se doit.
<img src="{{ '/assets/img/bf-pid.webp' | relative_url }}" alt="Onglet PID Tuning de Betaflight avec les sliders et les valeurs de PID" width="700">

### Filtres gyro
C'est la partie qui m'a demandé le plus d'allers-retours. Le filtre RPM est activé
(3 harmoniques, 120 Hz mini) puisque le DShot bidirectionnel remonte les régimes moteur,
ce qui permet de garder un filtrage assez léger ailleurs : un seul lowpass gyro en PT1
à 650 Hz et un notch dynamique entre 150 et 350 Hz. Multiplicateurs à 1.30 sur le gyro
et 1.10 sur le terme D — assez de marge pour éviter que les moteurs chauffent, sans
ajouter trop de latence.
<img src="{{ '/assets/img/bf-filter.webp' | relative_url }}" alt="Onglet Filter Settings de Betaflight, filtre RPM et notch dynamique activés" width="700">

### Rates
Rates en *Actual*, qui a l'avantage d'être lisible directement en degrés par seconde :
180 °/s de sensibilité au centre, 670 °/s en butée et 0,60 d'expo sur les trois axes.
Ça reste doux autour du neutre pour les lignes droites tout en laissant de quoi
enchaîner les flips.
C'est grâce au simulateur "The Zone" et au tuto de son développeur sur les rates que j'ai pû trouver les rates parfait pour mon pilotage
<img src="{{ '/assets/img/bf-rates.webp' | relative_url }}" alt="Onglet Rate Profile Settings de Betaflight avec des rates Actual" width="700">

### OSD
L'OSD est volontairement minimaliste : tension de la batterie, tension moyenne par
cellule, consommation en mAh, altitude, chronomètre de vol et l'alerte *LOW VOLTAGE*.
Unités en métrique, alarme capacité à 1300 mAh. En vol on n'a pas le temps de lire
quinze informations, donc tout ce qui n'est pas utile pour savoir quand rentrer a été
décoché.
<img src="{{ '/assets/img/bf-osd.webp' | relative_url }}" alt="Onglet OSD de Betaflight avec l'aperçu des éléments affichés" width="700">

### Divers
- Modes de vol : ACRO
- Blackbox activée pour relire les logs après les sessions de réglage

## Tests réalisés
- Test de continuité électrique
- Vérification du sens de rotation des moteurs
- Test du failsafe
- Premier vol stationnaire
- Tests de stabilité
- Ajustement des filtres et des PID

## Résultats
- Masse finale : 720g
- Autonomie en vol : environ 5 minutes en freestyle
- Comportement en vol : latence faible, bonne réactivité avec les moteur 1750KV meme si de part son poids, son inertie se fait ressentir
- Améliorations futures : Je pourrais modéliser et imprimer un support pour une caméra d'action afin d'avoir un enregistrement de mes vols en bonne qualité vidéo

