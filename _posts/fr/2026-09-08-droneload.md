---
layout: post
title: "DroneLoad : mener un projet de drone autonome à cinq"
ref: droneload
lang: fr
permalink: /journal/droneload/
status: "En cours"
cover: /assets/img/droneload.svg
cover_alt: "Photo de la cellule du drone DroneLoad en cours de montage"
cover_caption: "Remplacer par une photo de la cellule en cours de montage."
excerpt_text: "Parcours imposé, charge utile à transporter, cibles à détecter au sol. Je suis chef de projet sur DroneLoad et je m'occupe aussi de l'électronique embarquée. Voilà comment on a découpé le problème."
stack: ["ArduPilot", "Pixhawk", "C++", "Python"]
---

DroneLoad est une compétition étudiante de drone autonome. Le drone doit suivre un
parcours imposé sans pilote, transporter une charge utile et détecter des cibles au
sol. C'est mon projet de 4<sup>e</sup> année à l'ECE, on est une équipe de cinq et
j'en suis le chef de projet.

Deux casquettes, donc : coordonner, et tenir un lot technique. C'est un exercice
d'équilibre plus difficile que je ne le pensais.

## Découper le problème

La première semaine, on n'a pas touché au matériel. On a listé ce que la mission
exige réellement, puis on a découpé en quatre lots qu'une personne peut porter seule :

- **Cellule et propulsion** — choix du châssis, des moteurs et des hélices en
  fonction de la masse totale, charge utile comprise
- **Électronique embarquée et navigation** — contrôleur de vol, capteurs, firmware
- **Vision** — détection des cibles au sol
- **Mécanique de largage** — accroche et libération de la charge utile

Le point que j'avais sous-estimé : ces lots ne sont pas indépendants. La masse
retenue par le lot propulsion contraint directement la mécanique de largage, et le
budget énergie contraint tout le monde. On a fini par imposer un tableau partagé
avec masse, consommation et encombrement, mis à jour dès qu'une décision est prise.
Ce n'est pas glamour, mais c'est ce qui nous a évité deux ou trois allers-retours.

## Ma partie : électronique et navigation

Je m'occupe du contrôleur de vol et du firmware de navigation, sur base
Pixhawk / ArduPilot. Le choix d'ArduPilot plutôt que d'un stack maison n'a pas
vraiment été un débat : écrire un contrôleur d'attitude à partir de zéro nous
aurait pris le semestre entier, alors que la valeur du projet est dans la mission,
pas dans la boucle interne.

Ce qui reste à ma charge :

- la configuration et le calibrage complets de la centrale inertielle et du
  compas ;
- la définition du plan de vol et de la logique de mission ;
- la liaison entre le calculateur de vision et le contrôleur de vol, pour que la
  détection d'une cible déclenche une action ;
- les modes de repli : que fait le drone s'il perd le GPS, la liaison radio, ou les
  deux.

<!-- TODO : ajouter le schéma d'architecture (contrôleur de vol / calculateur vision / télémétrie) -->

## Ce qui n'est pas encore réglé

<!-- TODO : remplir au fur et à mesure — c'est la section qui rend le journal utile -->

À ce stade, deux questions ouvertes. La première est le compromis entre l'autonomie
et la charge utile : chaque gramme d'accus mange de la charge transportable, et on
n'a pas encore de mesure de consommation en vol pour trancher autrement qu'au doigt
mouillé. La seconde est la précision du positionnement au moment du largage, qui
conditionne le fait qu'on vise une zone ou un point.

Prochaine étape : premiers vols en mode stabilisé pour caractériser la
consommation réelle, avant de passer en mode autonome.
