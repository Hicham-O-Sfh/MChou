# M'Chou — prototype d'app mobile

Prototype interactif d'une app de commande/livraison de crêpes, brownies, choux, milkshakes et
mojitos à Oujda (Maroc). Exporté depuis **Claude Design** ; l'app finale sera développée en
**Flutter/Dart**. Ce dépôt sert à itérer sur le design (retouches, nouveaux écrans) avant le dev.

Méthodologie de travail détaillée : [.notes/methodologie.md](.notes/methodologie.md).

## Décisions en cours

- **Thème « Fidèle » (sombre) uniquement** pour l'instant. Le thème clair « Nutella Fun » est mis
  de côté (il reste dans `Explorations.dc.html` et `tokens/`) ; ne pas le maintenir en parallèle.
- **Images** : les placeholders `<image-slot>` suffisent. Pas d'upload. Si une vraie photo est
  nécessaire, elle est posée dans `assets/photos/` et référencée par l'attribut `src` du slot.
- **Réglages figés** : son activé, Reduce motion désactivé, LTR (voir `TW` dans le script).

## Fichiers

| Fichier | Rôle |
|---|---|
| `Ordering Prototype.dc.html` | Coquille du prototype : panneau de gauche, cadre téléphone, barre d'état, nav, sheets, toast, **toute la logique** |
| `screens/<id>.dc.html` | Template d'un écran (10 écrans : splash → track). Pas de logique |
| `Design System.dc.html` | Référence visuelle : couleurs, typo, formes, espaces, mouvement, icônes, composants |
| `Explorations.dc.html` | Comparaison statique des thèmes « Nutella Fun » (clair) et « Fidèle » (retenu) |
| `tokens/mchou.tokens.css` / `.json` | Tokens du design system (v0.2). **Non consommés par les pages** — voir plus bas |
| `support.js` | Runtime « dc » de Claude Design (généré). **Ne pas modifier** |
| `image-slot.js` | Composant `<image-slot>` (généré). **Ne pas modifier** |
| `assets/`, `uploads/` | Logo M'Chou (+ `assets/photos/` pour d'éventuelles photos) |
| `screenshots/` | Captures de vérification de la carte (export Claude Design) |
| `.notes/` | Notes de travail en Markdown (méthodologie, décisions, specs d'écrans) |

## Lancer le prototype

Les pages doivent être servies en HTTP (pas en `file://`) et ont besoin d'internet
(React 18 depuis unpkg, Google Fonts, Material Symbols, Leaflet + tuiles OSM pour les cartes).

- Dans Claude Code : config `prototype` de `.claude/launch.json` (port 8765).
- À la main : `python -m http.server 8765 --bind 127.0.0.1` puis
  `http://localhost:8765/Ordering%20Prototype.dc.html`.

Le 404 sur `.image-slots.state.json` est normal (fichier de persistance propre à Claude Design).

## Format `.dc.html`

Chaque page = un template dans `<x-dc>…</x-dc>` + (optionnel) un script de logique.

- `<helmet>` : contenu injecté dans le `<head>` (polices, scripts externes, CSS global).
- Liaisons `{{ expr }}` dans le texte et les attributs, résolues sur l'objet renvoyé par
  `renderVals()` du script.
- `<sc-if value="{{ cond }}">…</sc-if>`, `<sc-for list="{{ items }}" as="it">…</sc-for>`.
  Les attributs `hint-placeholder-*` ne servent qu'à l'aperçu dans l'éditeur Claude Design.
- `onClick="{{ handler }}"`, `ref="{{ fnRef }}"`, et `style-active="…"` (style appliqué au press).
- Styles **100 % inline** avec des valeurs en dur (hex, px), pas de classes ni de variables CSS.
- Logique : `<script type="text/x-dc" data-dc-script>` contenant `class Component extends DCLogic`
  (API type composant React : `state`, `setState`, `componentDidMount/DidUpdate`).
- `<dc-import name="chemin/sans-extension" dcProps="{{ obj }}"/>` insère un autre `.dc.html`
  (chargé au premier affichage) et lui passe toutes les clés de `obj` comme valeurs.

## Ordering Prototype : architecture

- **Coquille + écrans** : la coquille rend chaque écran via
  `<sc-if value="{{ is.<id> }}"><dc-import name="screens/<id>" dcProps="{{ all }}"/></sc-if>`.
  `renderVals()` expose toutes ses valeurs sous `all`, donc un écran utilise exactement les mêmes
  `{{ … }}` qu'avant le découpage.
- **Toute la logique reste dans la coquille** (un seul état partagé : panier, Miams, adresse…).
  Un écran n'a pas de script. Pour un nouvel écran : valeurs et handlers dans `renderVals()`,
  méthodes dans la classe, template dans `screens/<id>.dc.html`.
- Le nom passé à `dc-import` est encodé (`screens%2F<id>.dc.html`) ; le serveur Python le
  résout en sous-dossier. Un autre serveur doit faire de même.
- Les chemins (`src="assets/…"`) dans un écran sont relatifs à la **page racine**, pas à `screens/`.
- `onEnter()` attend que l'écran soit dans le DOM (fichier chargé au premier affichage) avant
  de lancer ses animations d'entrée.
- Écrans : racine `data-screen="<id>"` + `data-screen-label="NN Nom"`. Ordre dans `SCREENS`,
  aide du panneau gauche dans `HINTS`.
- Navigation : `this.go('<id>')`, `this.back()`, `this.soon('Nom')` pour un écran pas encore fait.
- Données de démo : produits `P`, catégories `CATS`, suppléments `EX`, état initial `state`.
- Feedback : `toastShow()`, `snd()` (sons WebAudio), `popEl/shake/burst/confetti`,
  sheets via `state.sheet` (rendues dans la coquille, au-dessus des écrans).
- Réglages : constante `TW = { sound, reduceMotion, rtl }`. `rm()` respecte aussi le réglage
  « réduire les animations » du système.
- Cadre téléphone 390×844 avec mise à l'échelle auto.

### Ajouter un écran

1. `screens/<id>.dc.html` : copier la structure d'un écran proche (racine
   `position:absolute;inset:0`, fond grain, ~54 px libres en haut sous la barre d'état, CTA en bas).
2. Coquille : `['<id>', 'Nom']` dans `SCREENS`, entrée dans `HINTS`, bloc `<sc-if>/<dc-import>`
   à côté des autres, valeurs dans `renderVals()`.
3. Brancher la navigation (remplacer le `this.soon(...)` correspondant).

## Conventions du design system (thème « Fidèle »)

Source de vérité : `Design System.dc.html` + `tokens/`. Résumé :

- **Couleurs** : fond plum 900 `#3A0B40`, cartes/inputs plum 800 `#4E1256`, barre plum 950
  `#24062A`, texte crème `#FFF6E9`, texte secondaire lilas `#E9D3EE`, nav inactive `#C9A9D1`,
  prix/actif/script banane `#FFD84D`. CTA = dégradé caramel `#FFD36B → #F7B538` avec lèvre
  `#C98A12`. Framboise `#FF4D6D` uniquement favoris/badges/promos flash ; erreurs `#FF6B85` ;
  succès pistache `#8FCB6B`.
- **Un seul CTA doré par écran** (plus les prix). Les éléments dorés reposent sur une « lèvre »
  de 4 px (box-shadow) pour paraître pressables.
- **Typo** : Pacifico (titres d'accent uniquement), Fredoka (titres, boutons, prix 700 18/22),
  Nunito (texte 500 16/24, petit 600 14/20, caption 800 12/16 en capitales +6 %).
  Fallback arabe : Baloo Bhaijaan 2.
- **Formes** : radius 12 / 20 / 28 / pill ; boutons pill ; cibles tactiles ≥ 48 px
  (CTA XL 64, L 56, M 48).
- **Espaces** : grille de 4 pt, gouttière 20.
- **Mouvement** : press 90 ms (scale .95, s'enfonce de 3 px), release 420 ms
  `cubic-bezier(.34,1.56,.64,1)`, count-up 600 ms, sheet 360 ms `cubic-bezier(.32,.72,0,1)`.
  Toujours respecter Reduce motion (fondu 150 ms) et le réglage son.
- **Icônes** : Material Symbols Rounded, poids 400, FILL 1 quand actif.
- **Barre de nav** : flottante à 16 px des bords, 5 entrées (Accueil, Menu, Panier surélevé
  de 34 px, Cadeaux, Profil).
- **RTL** : penser start/end, jamais left/right ; icônes directionnelles en miroir ; prix en
  chiffres latins + « DH ».
- **Ton** : français, tutoiement, court et gourmand (« Je me régale ! »). Pas de culpabilisation
  ni de fausse urgence.

## Règles de travail

- Les couleurs étant en dur dans les pages, un changement de couleur global se fait par
  remplacement dans **tous** les `.dc.html` (y compris `screens/`) **et** dans `tokens/`.
- Réutiliser les fragments HTML existants (boutons, cartes, chips, sheets) du Design System ou
  du prototype plutôt que réinventer un style.
- Après une modif, vérifier dans le navigateur (rendu + console) avant de conclure.
- Penser à la traduction Flutter : composants réutilisables, valeurs issues des tokens.
- Consigner décisions et specs dans `.notes/`.
