# Radio Española

Application web pour écouter les principales emisoras de radio espagnoles en direct.

**Démo** : `https://chrissorbadere.github.io/radio-espanola/` *(après déploiement)*

## Fonctionnalités

- **~100 emisoras espagnoles** chargées dynamiquement depuis l'API publique [Radio Browser](https://www.radio-browser.info/), triées par popularité réelle
- URLs **toujours à jour** (pas de maintenance manuelle des flux)
- **Mise en cache locale** (7 jours) : l'app fonctionne même hors-ligne après la première visite
- Stations en catalan automatiquement filtrées
- Recherche par nom ou genre
- Système de favoris avec persistance locale (`localStorage`)
- Lecteur fixe en bas d'écran, contrôle du volume sauvegardé
- Support des flux HLS (`.m3u8`) via hls.js pour Chrome/Firefox
- PWA installable sur mobile (Android / iOS) et desktop
- 100% statique, sans backend

## Déploiement GitHub Pages

1. Créer un nouveau repo, par exemple `radio-espanola`
2. Y déposer tous les fichiers de ce dossier à la racine
3. **Settings → Pages → Source** : `main` / `/ (root)`
4. Attendre 1-2 minutes : l'app est en ligne à `https://<utilisateur>.github.io/radio-espanola/`

## Architecture

L'app interroge au démarrage l'API Radio Browser :

- 4 serveurs de l'API essayés en cas d'indisponibilité (failover)
- Le paramètre `hidebroken=true` exclut les flux signalés comme cassés
- Le tri `clickcount` donne les emisoras les plus écoutées
- Filtrage côté client : exclusion langues/tags catalans, déduplication
- Résultat mis en cache 7 jours en `localStorage`

## Personnalisation

### Forcer un rechargement depuis l'API

Vider le cache navigateur, ou ouvrir la console et exécuter :
```javascript
localStorage.removeItem('radio-espanola.stations-cache'); location.reload();
```

### Modifier les filtres de langue

Dans `index.html`, repérer les constantes :
```javascript
const EXCLUDED_LANGUAGES = ['catalan', 'català', 'catalán'];
```

### Modifier les couleurs

Variables CSS dans `:root { ... }`. Changer `--accent` pour la couleur principale.

## Notes techniques

- Tous les flux doivent être en **HTTPS** (l'API Radio Browser fournit les versions HTTPS quand elles existent)
- Les flux HLS (`.m3u8`) sont gérés via [hls.js](https://github.com/video-dev/hls.js/)
- Si une emisora donne "Error de conexión", son flux est temporairement indisponible — la prochaine actualisation du cache (sous 7 jours) la remplacera automatiquement

## Crédits

- Liste des emisoras : [Radio Browser](https://www.radio-browser.info/) — base communautaire libre
- Lecteur HLS : [hls.js](https://github.com/video-dev/hls.js/)
