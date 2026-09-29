# Révisions Scolaires — Contexte projet

App web (fichier unique `index.html`, vanilla JS/CSS/HTML) pour aider un ado
de 12 ans à organiser et réviser ses cours : répétition espacée (SM-2),
gamification, import OCR de l'emploi du temps du collège. Hébergée sur
GitHub Pages (repo `mfnzt/revisions`), zéro backend, tout en LocalStorage.

Version courante : voir `const APP_VERSION` en tête du `<script>` dans
`index.html`. Historique détaillé : [CHANGELOG.md](CHANGELOG.md).
Fonctionnalités restantes : [ROADMAP.md](ROADMAP.md).

## Architecture

- Fichier unique `index.html` : HTML + CSS (tokens clair/sombre via
  `prefers-color-scheme`) + JS (classe `App`). SPA par toggle de classes
  CSS (`switchView`/`switchTab`), pas de framework, pas de build.
- Persistance : `localStorage` (clés `items`, `stats`), voir
  `App.loadData`/`saveData`.
- OCR : Tesseract.js uniquement (`window.ai` essayé puis retiré en v1.5.3,
  non supporté par les navigateurs testés). Flux en 2 phases séparées :
  **reconnaissance** (`parseOCRFormat`, pure, aucune notion de doublon)
  puis **ajout** (`addOCRCourses`, dédoublonnage intra-batch + contre
  l'existant, appelé seulement au clic sur "Ajouter").

## Modèle de données (`app.items`, 3 types)

| type | champs clés | cycle de révision |
|---|---|---|
| `exam` | `name`, `dueDate` | SM-2, disparaît une fois `dueDate` dépassée |
| `daily` (cours du jour) | `subject`, `content`, `sourceDate?` | SM-2 sans fin (1j/3j/7j/14-21j), visible dès le jour d'ajout |
| `homework` (DM) | `subject`, `content`, `dueDate`, `done`, `doneDate` | Pas de SM-2 — reste dû indéfiniment tant que `!done`, même en retard |

`createdDate` (ISO) sert au calcul SM-2, ne jamais y mettre une date texte.
`sourceDate` (sur `daily`/`homework` importés via OCR) sert uniquement au
dédoublonnage contre le texte OCR — ne pas confondre les deux.

## Invariants à ne pas casser

Découverts via des bugs réels déjà corrigés (détail dans CHANGELOG.md) :

- `addDays()` doit rester en UTC pur (`Date.UTC` + `setUTCDate`/`getUTCDate`)
  — mélanger heure locale et `toISOString()` décale tout d'un jour dans un
  fuseau en avance sur UTC (Europe/Paris).
- Tout nouvel item doit utiliser `generateId()` (UUID) — `Date.now()` seul
  collisionne quand plusieurs items sont créés dans la même boucle
  synchrone (import OCR en lot).
- `updateStreak()` ne doit se recalculer qu'une fois par jour calendaire
  (`stats.lastStreakCheckDate`), jamais à chaque navigation/reload.
- `switchView('dashboard')` doit appeler `app.render()` (pas juste
  `renderDashboard()`) pour rafraîchir points/streak/historique après
  une action.
- OCR : chaque carte importée est **devoir par défaut**
  (`isHomework = true`), case à cocher "C'est une leçon" pour
  l'exception. La date d'échéance d'un DM est **dérivée automatiquement**
  de la date OCR (`parseOcrDateToISO`), jamais redemandée à l'utilisateur.
- `checkDuplicate()` compare `item.sourceDate === course.date` (jamais
  `createdDate`, qui est la date d'AJOUT dans l'app, pas la date du cours).
