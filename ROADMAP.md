# Roadmap

État au 2026-09-29. Voir [CHANGELOG.md](CHANGELOG.md) pour ce qui est déjà livré.

## Phase 2 — non commencé

- **Notifications push** : aucune notification navigateur implémentée (ni
  demande de permission, ni Service Worker, ni rappel programmé à une
  heure fixe)
- **Graphique de progression** : aucune visualisation de
  l'historique/progression dans le temps
- **Édition/correction manuelle des cours OCR avant ajout** : l'aperçu
  OCR est en lecture seule (à part la case "C'est une leçon"),
  impossible de corriger un sujet/contenu mal reconnu avant d'ajouter

## Phase 3 — non commencé

- **Badges déblocables** ("Premiers pas", "Semaine dorée", "Expert") :
  aucun système de badges, seuls les compteurs points/streak existent
- **Export/Import des données (JSON)** : aucun moyen de
  sauvegarder/restaurer les données en dehors du LocalStorage du
  navigateur (risque de perte totale si cache vidé/navigateur changé)
- **Thème sombre avec bouton** : les couleurs dark existent déjà en CSS
  et suivent la préférence système (`prefers-color-scheme`), mais il n'y
  a aucun bouton pour basculer manuellement le thème (pas de vue
  "Paramètres" du tout)

## Autres manques identifiés (hors plan initial)

- Pas de moyen de modifier un cours/examen/DM après ajout (seulement
  suppression, depuis l'Historique)
- Pas de confirmation avant suppression d'un item dans l'Historique
- Pas de points/gamification accordés pour un DM marqué fait (choix
  actuel — ce n'est pas un exercice de mémorisation — à revoir si voulu)
