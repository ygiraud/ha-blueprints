# HANDOFF.md

## Last Agent

- Name: Claude
- Date: 2026-10-18
- Context: Audit de l'instance Home Assistant (2026.10.1), correction des blueprints chauffage et veilleuse, resync du depot avec HA.

---

## Objective

- Current goal: Garder le depot aligne sur les blueprints reellement utilises dans Home Assistant.
- Scope: Blueprints chauffage, veilleuse, lumiere presence, securite alarme ; README.md et HANDOFF.md.

---

## Completed Work

- ✅ Chauffage : suppression de `max(default=...)` (erreur `do_max`), `trigger_variables` pour `persons`
- ✅ Chauffage : condition anti-429 Netatmo (`netatmo_a_regler`, appel du script seulement si la consigne differe)
- ✅ Chauffage : absence mesuree depuis le dernier depart via `binary_sensor.<personne>_a_la_maison` (5 capteurs template crees dans HA)
- ✅ Chauffage : declencheurs separes depart / arrivee, trigger et condition `input_boolean.chauffage`
- ✅ Chauffage : `person.yoan` remplace par l'entrée `debug_notify_persons` (renseignee sur les 6 automations dans HA)
- ✅ Veilleuse : `color_temp_kelvin` au lieu de `kelvin`
- ✅ Resync depuis HA : `veilleuse.yaml`, `lumiere_auto_presence.yaml`, ajout de `securite_alarme_actions.yaml` et `securite_alarme_declenchement.yaml`
- ✅ README.md : ajout des 2 blueprints securite, prerequis et fonctionnement du chauffage et de la lumiere presence
- ✅ Scan donnees sensibles du depot : rien trouve

---

## Modified Files

- blueprint_chauffage_piece.yaml - anti-429, absence via capteurs `_a_la_maison`, entrée `debug_notify_persons`
- veilleuse.yaml - resync HA (`color_temp_kelvin`)
- lumiere_auto_presence.yaml - resync HA
- securite_alarme_actions.yaml, securite_alarme_declenchement.yaml - nouveaux, copies depuis HA
- README.md - documentation des blueprints et liens GitHub/raw
- HANDOFF.md - etat du travail realise

---

## Decisions

- Decision: Documenter chaque blueprint a partir de ses triggers, inputs et dependances reelles
  - Reason: Le README etait trop generique et omettait des prerequis importants
- Decision: Conserver les liens d'import `raw.githubusercontent.com` sur la branche `main`
  - Reason: C'est le format attendu par l'import Home Assistant pour les blueprints distants
- Decision: Retirer la mention `Reolink` du blueprint camera
  - Reason: Le blueprint repose sur une entité `camera` standard et n'est pas limite a cette marque
- Decision: Mesurer l'absence avec `binary_sensor.<personne>_a_la_maison` plutot que `person.*`
  - Reason: `last_changed` de `person.*` est remis a zero a chaque changement de zone
- Decision: Debug du chauffage via l'entrée `debug_notify_persons` (vide = desactive)
  - Reason: Ne plus coder `person.yoan` en dur dans le blueprint
- Decision: Comparer repo et HA semantiquement (YAML parse), pas en diff texte
  - Reason: HA reserialise le YAML des blueprints a la sauvegarde

---

## Open Questions / Risks

- 🔴 Blocking: Aucun
- 🟡 Risk: Le blueprint chauffage depend d'entites et scripts externes (`input_select.mode_de_maison`, `input_boolean.chauffage`, `script.set_netatmo_temperature_safe`, `notification_engine.create_event` en debug)
- 🟡 Risk: Non verifie en conditions reelles : absence apres un changement zone a zone, absence de 429 lors d'un vrai changement de consigne, comportement runtime de la nouvelle veilleuse
- 🟡 Risk: Capteurs `binary_sensor.<personne>_a_la_maison` crees dans HA uniquement (non versionnes)
- Question: Aucun

---

## Next Steps

- Immediate action: Relire le rendu Markdown sur GitHub
- Pousser `main` vers le remote quand l'utilisateur le demande (rien pousse a ce jour)
- Verifier la vanne de la chambre Evan (indisponible, a la charge de l'utilisateur)
- Suggested improvements: Ajouter plus tard un exemple de configuration ou une section compatibilite par blueprint si besoin
- Validation needed: Verifier que les boutons My Home Assistant importent bien chaque blueprint

---

## Notes

- Context: Depot de blueprints Home Assistant
- Assumptions: La branche publiee restera `main`
- Things to check: Compatibilite reelle des integrations SONOFF MQTT, Netatmo et NodOn dans l'instance cible
