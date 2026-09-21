---
layout: post
title: "DroneLoad #01 — Poser le cadre"
date: 2026-09-10
lang: fr
ref: droneload-01
categories: [droneload]
tags: [droneload, ardupilot, gestion-de-projet, journal-de-bord]
permalink: /journal/droneload-01-premiere-reunion/
cover: /assets/img/droneload.svg
cover_alt: "Le tableau de découpage des pôles lors de la première réunion"
excerpt_text: "Première semaine comme chef de projet : découpage en pôles, répartition de l'équipe, mise en place des outils."
---

## Le contexte

DroneLoad est notre projet de 4<sup>e</sup> année à l'ECE, une compétition étudiante de drone autonome. Le drone doit voler seul sur un parcours imposé, transporter une charge utile et détecter des cibles au sol. On est cinq sur le projet et j'en suis le chef.

Je démarre ce journal pour garder une trace de ce qu'on fait semaine après semaine. Un rapport final reconstruit tout après coup et lisse les erreurs ; je veux la version qui s'est vraiment passée, y compris ce qui coince. C'est aussi la première fois que je dirige une équipe. Je vais me tromper plusieurs fois, autant l'écrire au moment où ça arrive.

## Découper le projet

J'ai réuni l'équipe cette semaine. Objectif : personne ne sort de la salle sans savoir sur quoi il travaille.

On a d'abord listé au tableau tout ce que le drone doit savoir faire. Le découpage en quatre pôles est venu de cette liste, chaque pôle correspondant à un ensemble qu'une personne peut porter seule :

- **P1 — Mécanique, propulsion, largage** : concevoir la structure, dimensionner la chaîne propulsive, réaliser le mécanisme de largage, tenir le budget de masse. CAO, impression 3D, eCalc, atelier.
- **P2 — Avionique bas niveau** : faire voler le drone de façon stable et sûre. Contrôleur de vol Pixhawk 6 sous ArduPilot, calibrations, boucles PID, liaison MAVLink avec l'ordinateur compagnon.
- **P3 — Vision artificielle** : détecter la cible au sol depuis le drone et fournir à la navigation l'écart latéral en mètres. Python, OpenCV, marqueurs ArUco ou YOLO léger, optimisation sur Raspberry Pi ou Jetson.
- **P4 — Navigation sans GPS et ROS 2** : tenir la position sans GPS et piloter la mission. Fusion optical flow, LiDAR et EKF, architecture logicielle ROS 2, machine à états de la mission, simulation SITL et Gazebo.

Ces pôles ne sont pas indépendants. La masse que retient P1 contraint le largage, et le budget d'énergie contraint tout le monde. On tiendra donc un tableau commun de masse, de courant consommé et d'encombrement, mis à jour à chaque décision, sinon chaque pôle avance sur des hypothèses que les autres ignorent.

Pour la répartition, j'ai demandé à chacun sur quoi il voulait travailler et j'ai suivi les préférences. Je n'ai pas encore les critères pour juger du niveau de chacun, donc attribuer les pôles moi-même reviendrait à tirer au sort. Tout le monde démarre motivé. En contrepartie, la motivation ne garantit pas la compétence, et je ne le verrai qu'au bout de quelques semaines. Ce risque me paraît plus faible que celui de coller quelqu'un six mois sur un pôle qui ne l'intéresse pas.

On est deux sur P1, je suis seul sur P2, et j'ajoute la coordination par-dessus. P2 est ma zone de confort : Pixhawk, ArduPilot et boucles PID, c'est ce que je fais sur mes projets personnels depuis un moment, donc je peux en boucler le gros rapidement. P1 est l'engagement long : la mécanique et la propulsion vont bouger jusqu'au bout, au fil des essais en vol et des itérations sur le châssis et le largueur. Les trois autres se partagent P3 et P4.

## Ma place dans l'équipe

Je reste un étudiant d'ING4 comme eux, avec un pôle technique à tenir.

Concrètement, les décisions se discutent avant d'être prises, et une fois prises on avance sans les rouvrir. Sinon on repasse indéfiniment sur les mêmes sujets et plus rien n'avance. Je ne sais pas si le dosage est bon. Le risque que je vois déjà, c'est de ne pas réussir à imposer une décision impopulaire le jour où il faudra.

## L'intendance

Deux outils, en place dès cette semaine :

- un Drive partagé pour tout ce qui n'est pas du code : documents, CAO, comptes rendus de réunion, règlement du concours, photos ;
- un dépôt GitHub pour le code, organisé par pôle.

Si ce n'est pas posé en semaine 1, ça ne le sera jamais.

## Prise de contact

J'ai écrit au responsable du projet côté école et au responsable du concours. Deux questions : quel matériel est déjà disponible ou fourni, et quelles sont les dates réelles, jalons intermédiaires et date de compétition. Tant que je n'ai pas ces deux réponses, je ne peux ni construire un planning, ni chiffrer un budget, ni savoir si on part d'une base existante ou d'une feuille blanche.

Côté matériel, on récupère l'électronique du DroneLoad d'il y a deux ans : un contrôleur de vol Pixhawk 6, un module d'alimentation Holybro PM03D v1.1, quatre ESC T-Motor 20 A et quatre moteurs T-Motor 1000KV. Ça décharge une partie de P2 et ça allège le budget. Il faudra quand même tout vérifier et recalibrer après deux ans de placard, avant de considérer la base comme saine.

Cette base vient d'un autre projet, elle n'a pas été dimensionnée pour notre mission. Le courant admissible des ESC et le KV des moteurs contraignent le couple hélice/tension qu'on pourra utiliser, donc P1 doit valider la chaîne propulsive avec la masse en charge visée avant de dessiner quoi que ce soit. Si ça ne passe pas, c'est toute la propulsion qui repart en achat et le budget avec.

Le concours communiquera ses premières informations le 19 septembre. Le planning reste en suspens jusque-là.

## Ce qui m'inquiète

- Je ne connais pas le niveau technique réel de mes équipiers. Sur le papier tout le monde est partant, je le saurai au premier vrai livrable.
- Le planning reste vide tant que je n'ai pas les dates du concours.
- Je connais les références de l'électronique récupérée, pas son état ni ce qu'il manque autour. Le budget attend ça.
- Je n'ai jamais dirigé une équipe. Je ne sais pas ce que je fais mal en ce moment même.

## Objectifs de la semaine 2

- Réceptionner et inspecter le matériel récupéré : état des quatre moteurs et des quatre ESC, compatibilité du PM03D avec le Pixhawk 6.
- Récupérer les informations du concours le 19 septembre et en tirer un rétroplanning.
- Rédiger avec chaque pôle une fiche courte : objectif, livrables, premières échéances.
- Choisir l'architecture générale du drone : base de châssis, capteurs, ordinateur compagnon.
