# Prototype sur téléphone (à faire plus tard)

But : ouvrir la maquette interactive sur mon téléphone (Samsung Galaxy S25 FE, Android/Chrome),
seulement la partie téléphone, sans le panneau de gauche. Idée validée le 2026-10-15, pas encore
réalisée.

## 1. Mode téléphone dans la coquille

Dans `Ordering Prototype.dc.html`, actif sur écran étroit ou avec `?mobile` dans l'URL :

- masquer le panneau de gauche ;
- plus de cadre téléphone ni de mise à l'échelle 390×844 : l'app occupe tout l'écran ;
- masquer la fausse barre d'état (« 9:41 »), respecter les zones sûres
  (`env(safe-area-inset-*)`, `viewport-fit=cover`) ;
- bloquer le zoom au double-tap (`user-scalable=no`, `touch-action`).

## 2. Hébergement : GitHub Pages (piste préférée)

- HTTPS fourni, accessible partout (pas besoin du PC allumé).
- Internet reste nécessaire (React via unpkg, Google Fonts, Material Symbols, tuiles OSM).
- **Piège d'encodage** : le runtime charge les écrans via `dc-import name="screens/<id>"`, ce
  qui donne l'URL `screens%2F<id>.dc.html` (slash encodé). Notre serveur Python la résout en
  sous-dossier ; GitHub Pages probablement pas (à tester).
  - Renommer `Ordering Prototype.dc.html` en `Ordering-Prototype.dc.html` est bien (espace
    en moins dans l'URL), mais **ne règle pas** ce `%2F`.
  - Pistes : noms à plat sans slash (`screens-menu.dc.html` ou dossier supprimé), ou copie
    des fichiers sous le nom encodé au déploiement.
  - Penser à mettre à jour tous les `dc-import`, `CLAUDE.md` et `.claude/launch.json`.

## 3. « App » sur l'écran d'accueil (PWA)

Sur Android, l'installation en vraie PWA demande HTTPS : à combiner avec GitHub Pages.

- `manifest.json` (nom, icône M'Chou 192/512 px, `display: standalone`, couleurs `#3A0B40`) ;
- `<meta name="theme-color">` et lien vers le manifest dans le `<helmet>` ;
- Chrome propose « Installer l'application » ; un service worker n'est utile que si on veut
  le hors-ligne (pas prévu).

## Alternative rapide : même Wi-Fi

Serveur en `--bind 0.0.0.0`, autoriser Python dans le pare-feu Windows, ouvrir
`http://<IP-du-PC>:8765/…` sur le téléphone. Pas d'installation PWA (pas d'HTTPS), mais simple
pour tester.
