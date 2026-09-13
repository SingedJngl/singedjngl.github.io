---
layout: post
title: "Monter et régler un FPV 5 pouces en analogique"
ref: fpv-5in
lang: fr
permalink: /journal/drone-fpv-5-pouces/
cover: /assets/img/fpv.svg
cover_alt: "Le drone FPV 5 pouces terminé, posé sur un établi"
cover_caption: "Remplacer par une photo du drone terminé."
excerpt_text: "Un 5 pouces de course monté pièce par pièce, en vidéo analogique. Le montage n'est pas la partie difficile : le réglage et la propreté du câblage, si."
stack: ["Betaflight", "ESC 4-en-1", "Vidéo analogique"]
---

Ce drone est un cinq pouces de course classique, monté pièce par pièce, en
transmission vidéo analogique. J'aurais pu partir sur du numérique, mais l'analogique
reste moins cher, plus tolérant quand le signal se dégrade, et suffisant pour
apprendre.

## Le montage

<!-- TODO : lister précisément châssis / moteurs / ESC / FC / VTX / caméra -->

Rien de spectaculaire côté assemblage. Les deux choses qui font vraiment la
différence sur la durée :

- **La soudure.** Des soudures propres sur l'ESC et les moteurs, c'est ce qui évite
  les pannes intermittentes, celles qu'on ne reproduit jamais au sol.
- **Le cheminement des câbles.** Un fil qui traverse le châssis au mauvais endroit
  finit dans une hélice, ou bien injecte du bruit dans la ligne vidéo.

Cette deuxième leçon, je l'ai apprise en poursuivant pendant un moment des barres
parasites dans l'image, qui venaient simplement d'un câble d'alimentation passé trop
près du VTX.

## Le réglage sous Betaflight

C'est là que passe l'essentiel du temps. Le point de départ est toujours le même :
vérifier le sens de rotation des moteurs et le mapping des voies avant de toucher à
quoi que ce soit d'autre — un sens inversé se voit mal au sol et très bien au
décollage.

Ensuite, dans l'ordre :

1. **Filtrage.** Identifier les fréquences de vibration propres à la cellule, et
   filtrer juste ce qu'il faut. Trop peu, les moteurs chauffent ; trop, la réponse
   devient molle.
2. **PID.** Le réglage d'attitude proprement dit. On sent immédiatement le résultat,
   ce qui est très satisfaisant après des heures de filtrage.
3. **Rates.** La sensibilité des commandes, purement une affaire de goût de pilotage.

<!-- TODO : ajouter une capture Blackbox avant/après filtrage, c'est ce qui parle le mieux -->

## Ce que j'en retiens

Ce projet n'a rien inventé — les composants existent, le firmware existe. Ce qu'il
m'a apporté, c'est une intuition physique sur une boucle de régulation : comprendre
ce que « ça oscille » veut dire quand on le sent dans les manches, avant même de
regarder les courbes. Ça m'a nettement servi ensuite sur [DroneLoad](/journal/droneload/).
