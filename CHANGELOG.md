# Changelog

## v1.7.0 — Adaptation OCR au format carte de l'appli ado + fiabilisation
Le format de scan de l'appli ado (matière + badge Fait/Non Fait sur la même
ligne, checkbox "J'ai terminé" sur chaque carte, cartes sans consigne,
pièce jointe PDF référencée en texte) cassait le parsing prévu pour le
"format collège" initial :
- fix d'un bug latent où une ligne de statut isolée (`"Non Fait"`) pouvait
  être mal découpée par `courseRegex` en sujet="Non" + statut="Fait" —
  l'état isolé est désormais testé en priorité et attaché au cours en
  cours de construction au lieu d'être ignoré ;
- nouveau filtre pour la checkbox "J'ai terminé" (rendue de façon
  inconsistante par l'OCR: `"( J'ai terminé"`, `"@ ai terminé"`...),
  jamais confondue avec du contenu de cours ;
- une carte sans aucune consigne (cours vu, ex. "HISTOIRE-GEOGRAPHIE /
  Fait" sans texte) est importée avec un contenu placeholder plutôt
  qu'ignorée ;
- détection d'un mot-clé de contrôle (DS, contrôle, évaluation,
  interrogation...) en début de contenu → crée un item `exam` (cycle
  SM-2 jusqu'à l'échéance) au lieu d'un simple devoir ;
- le badge "Fait" du scan pré-remplit `done: true` / `doneDate` pour un
  devoir importé (nouveau paramètre optionnel sur `app.addHomework`) ;
- **validation stricte de la date d'échéance avant sauvegarde**
  (`isValidISODate`) : si `parseOcrDateToISO` échoue, la carte est
  clairement signalée "date non reconnue" dans l'aperçu et n'est jamais
  ajoutée avec une échéance invalide — auparavant le fallback d'affichage
  pouvait laisser croire qu'un texte brut était une date d'échéance.

## v1.6.1 — Inversion défaut DM/leçon en OCR
Chaque carte importée est devoir par défaut (case "C'est une leçon" pour
l'exception). Date d'échéance dérivée automatiquement de la date OCR
(nouvelle fonction `parseOcrDateToISO`), plus besoin de la saisir.

## v1.6.0 — Devoirs maison (DM) distincts des leçons
Nouveau type d'item `homework` : pas de cycle SM-2, reste dû tant qu'il
n'est pas coché fait (même en retard). Nouvel onglet "📝 Devoir",
bouton "Marquer comme fait" sur le tableau de bord, inclusion dans
l'Historique. Import OCR : case à cocher + date d'échéance par carte.

## v1.5.8 — Cours visibles le jour même de l'ajout
`getReviewsDue()` utilisait `createdDate+1j` comme 1ère disponibilité.
Changé pour `createdDate` (le jour même), suite à un effet de bord du
fix de fuseau horaire de la v1.5.7.

## v1.5.7 — Rafraîchissement UI + streak + fuseau horaire
`switchView('dashboard')` ne rafraîchissait pas points/streak/historique
après une révision (appelait `renderDashboard()` au lieu de `render()`).
Streak recalculé à chaque navigation au lieu d'1x/jour (gonflait à tort).
`addDays()` mélangeait heure locale et UTC, décalant toutes les dates
SM-2 d'un jour en fuseau français.

## v1.5.6 — IDs dupliqués sur import OCR en lot
`Date.now()` en résolution 1ms causait des ids identiques quand plusieurs
cours étaient ajoutés dans la même boucle synchrone → la popup "Réviser"
affichait toujours le même item. Fix : `generateId()` basé sur
`crypto.randomUUID()`.

## v1.5.5 — Bouton OCR bloqué + "undefined" sur description vide
`startOCR()` ne réinitialisait le bouton "Extraire" que sur erreur, jamais
sur succès → bloqué après la 1ère extraction réussie. `content` utilisait
`item.content || item.name`, affichant "undefined" quand `content` était
vide.

## v1.5.4 — Réécriture du parsing OCR + séparation reconnaissance/ajout
`parseOCRFormat()` réécrit en state machine basée sur le suffixe "Fait"/
"Non Fait" (au lieu d'une détection fragile par majuscules en début de
ligne, qui perdait des descriptions). Dédoublonnage déplacé entièrement
côté ajout (`addOCRCourses`), plus jamais à la reconnaissance. Fix du bug
racine des doublons : `checkDuplicate()` comparait `createdDate` (date
d'ajout) avec la date OCR — ajout du champ `sourceDate` dédié.

## v1.5.3 — Suppression de window.ai
`window.ai` non supporté par les navigateurs testés, retiré au profit de
Tesseract.js seul. Mode debug amélioré : affichage du texte OCR brut
complet (zone scrollable, copiable).

## v1.5.2 — Cache GitHub Pages + versioning dynamique
Meta tags no-cache pour forcer le rechargement sur GitHub Pages.
Affichage de version corrigé pour lire `APP_VERSION` dynamiquement au
lieu d'un texte figé dans le HTML.

## v1.5.1 — Amélioration détection de doublons OCR
`checkDuplicate()` comparait seulement sujet+date ; ajout de la
comparaison du contenu pour ne pas fusionner deux cours différents du
même jour et de la même matière.

## v1.5.0 — Import OCR
Tesseract.js + tentative window.ai, parsing du format collège
("Pour JOUR DATE" / barres colorées / matière / description / état
Fait-Non Fait), versioning affiché, mode debug initial.

## v1.0 — MVP
Formulaires Examen et Cours du jour, algorithme SM-2 (1j/3j/7j/14-21j),
gamification (points, streak), LocalStorage.
