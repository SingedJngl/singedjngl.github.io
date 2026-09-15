---
layout: post
title: "DroneLoad #01 — Poser le cadre"
date: 2026-09-14
lang: fr
ref: droneload-01
categories: [droneload]
tags: [droneload, ardupilot, gestion-de-projet, journal-de-bord]
image: /assets/img/droneload/01-cover.jpg
excerpt: "Première semaine comme chef de projet : découpage en pôles, répartition de l'équipe, mise en place des outils."
---

## Le contexte

DroneLoad, c'est notre projet de 4<sup>e</sup> année à l'ECE : une compétition étudiante de drone autonome. Le drone doit voler seul sur un parcours imposé, transporter une charge utile et détecter des cibles au sol. On est cinq sur le projet, et j'en suis le chef de projet.

Je démarre ce journal pour deux raisons. La première, c'est que je veux garder une trace de ce qu'on fait semaine après semaine — pas la version propre et reconstruite qu'on met dans un rapport final, mais ce qui s'est vraiment passé, y compris ce qui coince. La deuxième, c'est que c'est la première fois que je dirige une équipe. Je vais forcément me tromper plusieurs fois ; autant l'écrire au moment où ça arrive plutôt que de le romancer après coup.

## Découper le projet

Première réunion d'équipe cette semaine. Mon objectif était simple : ne pas sortir de la salle sans que chacun sache sur quoi il bosse.

J'ai commencé par découper le projet en pôles. Une fois qu'on avait listé au tableau tout ce que le drone doit savoir faire, le découpage est venu assez naturellement :

- **P1 — Mécanique, propulsion, largage** : concevoir la structure du drone, dimensionner la chaîne propulsive, réaliser le mécanisme de largage de la charge utile, tenir le budget de masse. CAO, impression 3D, eCalc, atelier.
- **P2 — Avionique bas niveau** : faire voler le drone de façon stable et sûre. Contrôleur de vol (Pixhawk / ArduPilot), calibrations, boucles d'asservissement PID, liaison MAVLink avec l'ordinateur compagnon.
- **P3 — Vision artificielle** : détecter la cible au sol depuis le drone et fournir à la navigation l'écart latéral en mètres. Python, OpenCV, marqueurs ArUco ou YOLO léger, optimisation sur Raspberry Pi ou Jetson.
- **P4 — Navigation sans GPS & ROS 2** : tenir la position sans GPS et piloter la mission. Fusion de capteurs (optical flow + LiDAR + EKF), architecture logicielle ROS 2, machine à états de la mission, simulation SITL + Gazebo.

Pour la répartition, j'ai fait un choix assumé : plutôt que d'attribuer les pôles moi-même sur des critères que je n'ai pas encore, j'ai demandé à chacun sur quoi il avait envie de travailler et j'ai réparti selon les préférences. L'avantage, c'est que tout le monde démarre motivé. L'inconvénient, c'est que la motivation ne garantit pas la compétence, et je ne le verrai qu'au bout de quelques semaines. Je préfère ce risque-là à celui de coller quelqu'un sur un pôle qui ne l'intéresse pas pendant six mois.

Résultat : on suis est à deux sur P1 et je suis seul sur P2, en plus de la coordination. Dit comme ça, ça fait beaucoup, mais en pratique P2 est ma zone de confort; Pixhawk, ArduPilot, boucles PID, c'est ce que je fais depuis un moment avec mes projets perso, et je pense pouvoir boucler le gros du travail assez vite. Le vrai engagement long terme, c'est P1 : la mécanique et la propulsion vont évoluer jusqu'au bout du projet, au fil des essais en vol et des itérations sur le châssis et le largueur. Les trois autres se partagent P3 et P4.

## Ma place dans l'équipe

Un truc que j'ai essayé de tenir dès la première réunion : rester à la même hauteur que les autres. Je suis chef de projet, mais je reste un étudiant de 4A comme eux, avec un pôle technique à assumer. Je n'ai pas envie du mode « je distribue les tâches et je supervise ».

Concrètement, ça veut dire que les décisions se discutent avant d'être prises, et qu'une fois prises, on avance sans les rouvrir. Est-ce que c'est le bon dosage ? Je n'en sais rien. Le risque que je vois déjà, c'est de ne pas arriver à imposer une décision impopulaire le jour où il faudra le faire.

## L'intendance

Simple :

- un **Drive partagé** pour tout ce qui n'est pas du code : documents, CAO, comptes rendus de réunion, règlement du concours, photos ;
- un **dépôt GitHub** pour l'ensemble du code, avec une organisation par pôle.

Si ce n'est pas en place dès la semaine 1, ça ne le sera jamais.

## Prise de contact

J'ai aussi écrit au responsable du projet côté école et au responsable du concours. Deux questions principales : **quel matériel est déjà disponible ou fourni**, et **quelles sont les dates réelles** — jalons intermédiaires, date de la compétition. Ça paraît trivial, mais tant que je n'ai pas ces deux réponses, je ne peux ni construire un planning, ni chiffrer un budget, ni savoir si on part d'une base existante ou d'une feuille blanche.

Côté matos, bonne nouvelle, on récupère l'électronique du projet DroneLoad d'il y a deux ans càd le contrôleur de vol, l'ESC et quatre moteurs. On ne part donc pas de zéro sur la partie avionique, ce qui me décharge un peu sur P2. Il faudra quand même tout vérifier, recalibrer et s'assurer que tout reste fonctionnel après être resté dans un placard pendant deux ans, mais c'est toujours ça de gagné sur le budget et le délai.

Pour les dates et le règlement détaillé, le concours nous communiquera les premières informations le 19 septembre. D'ici là, le planning reste en suspens.

## Ce qui m'inquiète

Pour être honnête, je termine la semaine avec plus de questions que de réponses :

- Je ne connais pas encore le niveau technique réel de mes équipiers. Sur le papier tout le monde est partant ; je ne saurai ce que ça donne qu'au premier vrai livrable.
- Le planning reste vide tant que je n'ai pas les dates du concours.
- On a récupéré la base électronique, mais je ne sais pas encore dans quel état elle est ni ce qu'il manque autour — donc toujours pas de budget.
- Et surtout; je n'ai jamais dirigé une équipe. Je n'ai aucune idée de ce que je fais mal en ce moment même.

La semaine 1 ne sert pas à avoir des réponses, elle sert à poser un cadre dans lequel les réponses pourront arriver. L'équipe existe, les pôles existent, les outils existent, les bonnes questions sont posées aux bonnes personnes. C'est déjà ça.

## Objectifs de la semaine 2

- Réceptionner et inspecter le matériel récupéré (FC, ESC, moteurs) — vérifier l'état et la compatibilité.
- Récupérer les premières infos du concours le 19 septembre et en tirer un rétroplanning.
- Rédiger avec chaque pôle une courte fiche : objectif, livrables attendus, premières échéances.
- Commencer à choisir l'architecture générale du drone (base de châssis, capteurs, ordinateur compagnon).
