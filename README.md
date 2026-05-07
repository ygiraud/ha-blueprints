# Home Assistant Blueprints

Collection de blueprints Home Assistant pour des automatisations domestiques simples, lisibles et réutilisables.

Depot GitHub : `git@github.com:ygiraud/ha-blueprints.git`

## Installation

Cliquez sur le bouton d'import du blueprint voulu. Si Home Assistant est deja ouvert sur votre appareil, la fenetre d'import sera pre-remplie automatiquement.

## Blueprints disponibles

### Interrupteur simple lumière

Bascule une lumiere via un appui `toggle` sur un interrupteur SONOFF expose comme appareil MQTT.

Pré-requis :
- un appareil SONOFF remonte dans Home Assistant
- une entité `light`

Fichier source : [`interrupteur.yaml`](https://github.com/ygiraud/ha-blueprints/blob/main/interrupteur.yaml)

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/ygiraud/ha-blueprints/main/interrupteur.yaml)

### Interrupteur double

Pilote deux lumieres distinctes avec les actions `toggle_l1` et `toggle_l2` d'un interrupteur SONOFF double expose comme appareil MQTT.

Pré-requis :
- un appareil SONOFF double remonte dans Home Assistant
- deux entités `light`

Fichier source : [`interrupteur double.yaml`](https://github.com/ygiraud/ha-blueprints/blob/main/interrupteur%20double.yaml)

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/ygiraud/ha-blueprints/main/interrupteur%20double.yaml)

### Veilleuse

Allume une lumiere uniquement la nuit sur detection de mouvement, avec luminosite configurable, temperature de couleur fixe a `2000 K` et extinction apres retour a l'absence puis delai configurable.

Pré-requis :
- un capteur `binary_sensor` de type `occupancy` ou `motion`
- une entité `light`

Fichier source : [`veilleuse.yaml`](https://github.com/ygiraud/ha-blueprints/blob/main/veilleuse.yaml)

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/ygiraud/ha-blueprints/main/veilleuse.yaml)

### Lumière automatique sur présence

Allume la lumiere a la detection de presence et l'eteint a la disparition de presence.

Pré-requis :
- un capteur `binary_sensor` avec `device_class: occupancy`
- une ou plusieurs entités `light`

Fichier source : [`lumiere_auto_presence.yaml`](https://github.com/ygiraud/ha-blueprints/blob/main/lumiere_auto_presence.yaml)

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/ygiraud/ha-blueprints/main/lumiere_auto_presence.yaml)

### Reolink - Snapshot et notification mouvement

Prend un snapshot d'une camera puis cree un evenement `Notification Engine` avec image lorsqu'un des capteurs de detection associes passe a `on`.

Fonctionnement :
- le type d'evenement est deduit du nom du capteur declencheur : personne, vehicule, animal, mouvement ou activite
- l'envoi est conditionne par l'etat d'un select de mode hub
- au moins une entité `person` doit etre selectionnee pour recevoir la notification

Pré-requis :
- une entité `camera`
- un ou plusieurs `binary_sensor` de detection associes
- une ou plusieurs entités `person`
- une entité `select` pour le mode hub
- le service `notification_engine.create_event`
- ecriture possible dans `/config/www/` pour stocker le snapshot

Fichier source : [`mouvement_camera.yaml`](https://github.com/ygiraud/ha-blueprints/blob/main/mouvement_camera.yaml)

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/ygiraud/ha-blueprints/main/mouvement_camera.yaml)

### Chauffage par pièce

Pilote le chauffage d'une piece en combinant :
- des entités `climate` Netatmo via une temperature de consigne
- des entités `select` NodOn via un mode fil pilote
- la presence, les fenetres ouvertes et des plages horaires semaine / week-end

Le blueprint reevalue son etat toutes les 30 minutes et sur les changements de presence, de mode maison, de fenetres et d'horaires.

Pré-requis :
- Home Assistant `2024.6.0` minimum
- au moins un chauffage Netatmo (`climate`) et/ou NodOn (`select` MQTT)
- si aucune personne n'est configuree, l'entité `input_select.mode_de_maison`
- le script `script.set_netatmo_temperature_safe` pour les chauffages Netatmo
- optionnel : `input_boolean.debug` et `notification_engine.create_event` pour les notifications de debug

Fichier source : [`blueprint_chauffage_piece.yaml`](https://github.com/ygiraud/ha-blueprints/blob/main/blueprint_chauffage_piece.yaml)

[![Importer dans Home Assistant](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/ygiraud/ha-blueprints/main/blueprint_chauffage_piece.yaml)

## Import manuel

Si le bouton d'import direct ne fonctionne pas, copiez l'URL brute du fichier YAML voulu puis importez-la depuis :

`Parametres > Automatisations et scenes > Blueprints > Importer un blueprint`
