# Documentation — Futures mises à jour

Ce document décrit la stratégie et les actions recommandées pour les futures mises à jour du projet « PDF Insight AI » (repo: eliendombe/pdf_charge_search). Il sert de guide pour les releases, priorisation, modèles de changelog, checklist de publication et tickets actionnables.

## Résumé rapide
- Projet : PDF Insight AI — analyse locale de fichiers PDF (frontend uniquement, PDF.js)
- Objectif du document : définir les futures évolutions, bonne pratique de release, et fournir des templates réutilisables.

---

## Roadmap priorisée
### Court terme (0–4 semaines)
- Ajouter un fichier CHANGELOG.md et adopter SemVer (ex: 0.1.0).
- Améliorer l'UX d'extraction : barre de progression, toasts visuels au lieu de console.log, messages d'erreur clairs.
- Extraire les fonctions pures (findBestMatch, generateSummary, escapeHtml) dans un module `src/search.js` pour faciliter les tests.
- Ajouter tests unitaires (Jest ou Mocha) pour ces fonctions.
- Ajouter un serveur local simple dans README (ex: `python3 -m http.server 8000`).

### Moyen terme (1–3 mois)
- Remplacer le scoring basique par un index léger (Fuse.js) ou TF-IDF pour recherche fuzzy.
- Ajouter option "mode hors-ligne complet" : inclure PDF.js et Font Awesome localement et ajouter manifest/service-worker (PWA).
- Ajouter export des résultats (JSON / TXT / PDF).
- Mettre en place CI (GitHub Actions) pour lint & tests.

### Long terme (optionnel)
- Support OCR (Tesseract.js) pour PDF scannés.
- Option d'intégration LLM local (via embeddings/outil local) — documenter clairement l'impact confidentialité.
- Internationalisation (i18n) et amélioration a11y (lecteurs d'écran, navigation clavier complète).

---

## Modèle de CHANGELOG (KEEP A CHANGELOG)
Créez un fichier `CHANGELOG.md` à la racine et utilisez ce template :

```
# Changelog
Toutes les modifications notables à ce projet seront documentées ici.

## [Unreleased]

### Added
- 

### Changed
- 

### Fixed
- 

## [0.1.0] - YYYY-MM-DD
### Added
- Version initiale : index.html, styles.css, script.js, README.md
```

---

## Checklist de release (avant tag)
- [ ] Mettre à jour `CHANGELOG.md` (déplacer Unreleased -> version)
- [ ] Bump version (SemVer) et tag git
- [ ] Exécuter tests unitaires et lint
- [ ] Tester manuellement sur Chrome/Firefox/Safari mobile
- [ ] Vérifier accessibilité basique (contraste, navigation clavier)
- [ ] Valider CSP si vous restaurez le mode offline
- [ ] Créer la Release GitHub avec notes (résumé, breaking changes, auteurs)

---

## Templates utiles
### Issue — Bug report (suggestion de contenu)
```
Titre: [Bug] Titre court
Description:
- Steps pour reproduire:
  1. Ouvrir ...
  2. Charger fichier ...
- Comportement attendu:
- Comportement observé:
- Environnement: navigateur, version, OS
- Fichier PDF de test (si possible)
```

### Issue — Feature request
```
Titre: [Feature] Brève description
Contexte / besoin:
Proposition:
Bénéfice:
Critères d'acceptance:
```

---

## Suggestions techniques concrètes
- Search/indexing
  - Intégrer Fuse.js (fuzzy search) : plus tolérant aux fautes et facile à intégrer côté client.
  - Ou implémenter TF-IDF simple en JS si vous souhaitez contrôler la pondération.
- Architecture code
  - Déplacer la logique non-UI de `script.js` vers `src/` (ex: `src/pdf.js`, `src/search.js`, `src/ui.js`).
  - Rendre `script.js` une simple orchestration qui charge ces modules.
- Tests
  - Ajouter Jest (ou Mocha) et tests pour `findBestMatch`, `generateSummary`, `escapeHtml`.
- Sécurité / CSP
  - Pour mode offline : héberger `pdf.min.js` et `pdf.worker.min.js` localement et adapter la meta CSP.
  - Ajouter SRI (integrity) pour CDN si vous conservez CDN.
- Performance
  - Chunker l'extraction pour gros PDF (processing par page, indexation progressive).

---

## Priorités de PR (3 propositions immédiates)
1. PR #1 — Ajouter `CHANGELOG.md` + badge version dans README. (Très haute priorité)
2. PR #2 — Extraire `findBestMatch` dans `src/search.js` et ajouter tests unitaires.
3. PR #3 — Ajouter GitHub Action minimal `lint-and-test.yml` et badge CI dans README.

---

## Conventions de commit recommandées
Utiliser Conventional Commits pour faciliter la génération de changelogs automatiques:
- feat(scope): description
- fix(scope): description
- docs: description
- chore: description

Ex: `feat(search): add fuzzy search with Fuse.js`

---

## Exemple de message de release (template)
```
Title: vMAJOR.MINOR.PATCH — YYYY-MM-DD
Body:
- Résumé court des changements
- Added: ...
- Changed: ...
- Fixed: ...
- Notes de migration (si breaking)
```

---

## Prochaines actions que je peux faire pour vous
- Créer ce fichier `FUTURES_MISES_A_JOUR.md` (je le fais maintenant) — déjà en cours.
- Créer aussi un `CHANGELOG.md` et des templates d'issues / workflows si vous le souhaitez.

Si vous voulez d'autres noms de fichier (par ex. `CHANGELOG.md` ou `ROADMAP.md`) dites-le, je peux les ajouter.
