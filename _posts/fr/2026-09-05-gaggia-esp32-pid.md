---
layout: post
title: "Reprendre le contrôle d'une Gaggia Classic avec un ESP32"
ref: gaggia-pid
lang: fr
permalink: /journal/gaggia-esp32-pid/
status: "En cours"
cover: /assets/img/gaggia.svg
cover_alt: "La Gaggia Classic ouverte, avec la carte ESP32 posée à côté"
cover_caption: "Remplacer par une photo de la machine ouverte, carte visible."
excerpt_text: "D'origine, une Gaggia Classic régule sa température avec un thermostat bilame : environ ±10 °C autour de la consigne. J'ai remplacé ça par une boucle PID sur ESP32, avec une sonde PT1000 et un relais statique."
stack: ["ESP32", "MAX31865", "PT1000", "PID", "MQTT", "Grafana"]
---

La Gaggia Classic est une machine à espresso à chaudière unique, réputée solide et
facile à ouvrir. Son point faible est connu : la température est régulée par un
thermostat bilame, qui ouvre et ferme le circuit chauffant sur une plage de l'ordre
de ±10 °C. Pour de l'eau chaude c'est sans conséquence ; pour une extraction, ça ne
l'est pas — la température de contact influe directement sur ce qui se retrouve dans
la tasse.

L'idée du projet est donc simple à énoncer : mesurer correctement, et piloter
correctement.

## La chaîne de mesure

Le thermostat d'origine est remplacé par une sonde **PT1000** vissée sur la
chaudière, lue par un **MAX31865** en SPI. Deux raisons pour ce choix plutôt qu'un
thermocouple : la PT1000 est plus linéaire sur la plage qui m'intéresse (90–150 °C),
et le MAX31865 gère le montage en trois fils, ce qui compense la résistance des
câbles.

Point d'attention, appris à mes dépens : la sonde mesure la température du corps de
chaudière, pas celle de l'eau qui traverse le groupe. Il y a un écart, et il n'est
pas constant — il dépend du temps écoulé depuis la dernière extraction.

<!-- TODO : mettre la courbe relevée température chaudière vs température en sortie de groupe -->

## La boucle de régulation

Le corps de chauffe est piloté par un **relais statique**, commandé par l'ESP32.

Quelques points que je n'avais pas anticipés en écrivant le PID :

- **Anti-windup.** À la mise sous tension, l'écart est énorme et le terme intégral
  se charge pendant toute la montée en température. Sans saturation du terme I, la
  machine dépasse largement la consigne avant de redescendre.
- **Double consigne.** Café et vapeur ne demandent pas la même température ; le
  passage de l'une à l'autre est une transition à gérer explicitement, pas un simple
  changement de variable.
- **Watchdog logiciel.** Un firmware qui plante en laissant le relais fermé, sur une
  résistance chauffante, ce n'est pas un bug anodin. Le watchdog coupe le chauffage
  si la boucle ne s'exécute plus.

<!-- TODO : préciser les gains Kp/Ki/Kd retenus et la méthode de réglage -->

## Instrumenter l'extraction

Au-delà de la température, j'ajoute un capteur de pression et un débitmètre
échantillonnés pendant l'extraction. L'objectif n'est pas le pilotage en boucle
fermée — pas encore — mais l'observation : pouvoir comparer deux extractions et
comprendre ce qui a changé.

La télémétrie part en **MQTT** et se visualise dans **Grafana**. C'est
surdimensionné pour une machine à café, je l'assume : c'était aussi l'occasion de
manipuler proprement une chaîne de collecte.

## Ce qui reste

Le firmware tourne sous framework Arduino. Reste à faire : profil d'extraction
reproductible, et une interface un peu moins rudimentaire que la liaison série.
