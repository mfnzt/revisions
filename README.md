# 📚 Révisions Scolaires

Une application web moderne pour aider les ados à réviser intelligemment avec :
- **Répétition espacée (SM-2)** : Algorithme scientifique pour mémoriser
- **Gamification** : Points, streaks, badges pour la motivation
- **Import OCR** : Capture d'écran de l'emploi du temps → extraction automatique
- **100% Navigateur** : Aucun backend requis, données en LocalStorage

## 🚀 Démarrage rapide

1. Ouvrez `index.html` dans un navigateur moderne
2. Ajoutez vos matières à réviser
3. Complétez les révisions quotidiennes
4. Consultez votre progression

## 📋 Phases

### Phase 1 ✅
- MVP : Formulaires + SM-2 + Gamification
- Modes : Examen (avec date) et Cours du jour

### Phase 1.5 ✅
- OCR : window.ai + Tesseract fallback
- Import automatique depuis captures d'écran

### Phase 2 ⏳
- Notifications push
- Graphique de progression

### Phase 3 ⏳
- Badges déblocables
- Export/Import données
- Thème dark

## 🛠️ Tech Stack

- **Frontend** : HTML5 + CSS3 + JavaScript vanilla
- **Storage** : LocalStorage (~10MB)
- **OCR** : window.ai (priorité) + Tesseract.js (fallback)
- **Framework** : Aucun (zéro dépendances)

## 📱 Compatibilité

- Chrome/Edge (pour window.ai)
- Firefox, Safari (Tesseract fallback)
- Mobile ready

## 📖 Docs

Voir `/.claude/plans/` pour le plan détaillé d'implémentation.

---

**Créé pour aider à réviser intelligemment ! 🎯**
