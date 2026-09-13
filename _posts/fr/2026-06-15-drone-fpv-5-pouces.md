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
- Configuration des UART
- Système vidéo : analogique, émetteur piloté en IRC Tramp
- Protocole ESC : DShot 600
- Protocole radio : CRSF

## Réglages
- Modes de vol : ACRO
- Failsafe étapes 1 et 2 configurées
- Réglage des PID
- Filtres gyro
- Rates
- OSD
- Blackbox

## Tests réalisés
- Test de continuité électrique
- Vérification du sens de rotation des moteurs
- Test du failsafe
- Premier vol stationnaire
- Tests de stabilité
- Ajustement des filtres et des PID

## Problèmes rencontrés
- Bruit vidéo
- Solutions mises en œuvre

## Résultats
- Masse finale :
- Autonomie en vol :
- Comportement en vol :
- Améliorations futures :

## Médias
Ajouter ici les photos, schémas de câblage ou vidéos.

