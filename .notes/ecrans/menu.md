# Écran Menu

Source : photos de la carte papier M'Chou (3 volets), 2026-10-14.

## But

Parcourir toute la carte (~115 produits, 13 familles) et ajouter au panier en 1 à 2 taps,
sans se perdre. Accès : onglet **Menu** de la barre du bas, et « Tout voir » sur Home.

## Idée directrice

La carte papier est organisée **par famille** (crêpes, gaufres, choux…), mais les clients
pensent souvent **par saveur** : les mêmes parfums reviennent partout (Nutella, Pistache, Lotus,
Bueno, Oreo, Amande). L'écran propose donc deux façons de chercher, sur une seule page :

1. **Par famille** : une barre de familles collante en haut, synchronisée avec le défilement
   (tap = saut vers la section, défilement = la famille active s'allume).
2. **Par saveur** : une rangée de pastilles « Ta saveur ? » qui filtre toute la carte
   (ex. « Pistache » → crêpes, gaufres, choux, brownies… à la pistache).

Plus la recherche texte, comme sur Home.

## Structure (de haut en bas)

- En-tête : titre « Le menu », sous-titre en Pacifico, champ de recherche.
- En-tête collant (`data-menu-stick`) : pastilles de saveurs + barre de familles restent
  ensemble en haut au défilement ; seul le titre et la recherche défilent.
- Pastilles de saveurs (filtre, une seule active, re-tap = désactive).
- Barre de familles, avec les icônes Material déjà prévues dans le design system :
  skillet, dinner_dining, breakfast_dining, bakery_dining, cake, icecream, local_bar,
  nutrition, blender, local_cafe, takeout_dining.
- Une section par famille : titre en Pacifico (clin d'œil à la carte papier) + lignes compactes.
- Ligne produit : vignette ronde teintée avec icône (pas de photo), nom, détail court
  (« 4 ou 6 pièces », « M ou L »), prix doré (« dès 12 DH » s'il y a des variantes), bouton « + ».
- Packs famille mis en avant (carte dorée, icône cadeau, comme sur la carte papier).
- En bas : carte « Nos services » (fêtes, soutenances, commandes perso) → contact Instagram.
- Barre du bas visible, onglet Menu actif.

## Ajout au panier

- « + » ajoute directement la variante par défaut, le bouton devient un stepper (comme Home).
- Tap sur une douceur (crêpes, spaghetti, gaufres, brownies, choux) → **builder**
  (voir [builder.md](builder.md)). Tap sur une boisson ou un pack → **feuille d'options** :
  - pièces (4 / 6), façon des choux (classiques / farcis), taille (M / L), jus nature / au lait,
    en sélecteurs avec le prix de chaque choix ;
  - crème (vanille / chocolat) pour les choux farcis ;
  - arôme +5 DH (vanille / caramel / noisette) pour les boissons chaudes ;
  - suppléments (liste réelle) pour crêpes, gaufres, brownies et choux ;
  - quantité + CTA doré « Ajouter · 30 DH » (le prix se recalcule en count-up) → vol vers le panier.

## États

- Recherche ou saveur sans résultat : message + suggestion.
- Produit épuisé : grisé, « Épuisé · retour demain ».
- Filtre saveur actif : les familles vides disparaissent de la barre.

## Carte retranscrite

Prix en DH. « a / b » = deux variantes.

### Crêpes sucrées
Nutella 20 · Nutella banane 25 · Oreo 25 · Bueno 25 · Lotus 25 · Amande 25 ·
Nutella crème pistache 25 · Nutella pistachio 30 · Extra amande pistachio 30 ·
Croquant amande 35 · Extra pistachio 35 · Filet d'amande 35

### Spaghetti crêpes
Nutella 25 · Nutella banane 30 · Oreo 30 · Bueno 30 · Lotus 35 · Amande 35 ·
Nutella crème pistache 35 · Nutella pistachio 40 · Croquant amande 40 · Extra pistachio 45 ·
Filet d'amande 40

### Crêpes spéciales
Kunafa pistachio burritos 38 · Brownie chocolat 35 · Corne de gazelle 45 · Brownie dubaï 50 ·
Box mixte 35 · Enfant 35

### Gaufres croustillantes
Natural 15 · Nutella 20 · Nutella banane 25 · Oreo 25 · Lotus 25 · Bueno 25 · Amande 25 ·
Nutella crème pistachio 25 · Nutella pistachio 30 · Croquant amande 35 · Box mixt 35 ·
Extra pistachio 40

### Suppléments
Banane 5 · Fraise 8 · Crème chantilly 8 · Pistache 10 · Amande 8 · Lotus 8 · Oreo 5 ·
Kunafa pistache 15

### Choux (4 / 6 pièces)
Nutella 12/15 · Bueno 13/16 · Lotus 16/20 · Oreo 15/20 · Croquant amande 20/25 ·
Pistache 20/25 · Amande 16/20 · Box mixt 20/25

### Choux farcis, crème au choix vanille / chocolat (4 / 6 pièces)
Nutella 18/24 · Bueno 20/25 · Lotus 22/27 · Oreo 20/26 · Croquant amande 30/35 ·
Pistache 25/30 · Amande 22/27 · Box mixt 25/30

### Brownies, kunafa pistachou & crème fromagère (M / L)
Nutella 27/49 · Bueno 28/50 · Nutella pistachou 30/55 · Nutella crème pistachio 28/50 ·
Amande 30/55 · Croquant amande 35/55 · Lotus 30/55 · Oreo 30/55 · Extra pistachio 35/59 ·
Box mixt 30/55

### Packs famille
Choux : 24 pièces mixtes 93 · 12 pièces « brisée » 190 (à vérifier)
Brownies : 12 pièces mixtes 149 · 12 pièces extra pistachou 155

### Milkshakes
Vanille, Fraise, Framboise, Banane, Chocolat, Caramel, Oreo : 27

### Mojitos
Citron, Fraise, Framboise, Blueberry, Fruits de la passion, Ananas : 20

### Jus
Citron 15 · Orange 20 · Pomme 15/25 · Banane 15/25 · Fraise 23/28 · Avocat 25/30 ·
Ananas 28 · Mangue 28 · Kiwi 28

### Cocktails
Hawaï (mangue, fraise) 45 · Bora Bora (banane, pomme) 35 · Île Maurice (ananas, kiwi) 45 ·
Île Fraîcheur (ananas, citron, sirop de menthe) 45
(Mangue 28 et Kiwi 28 apparaissent aussi sous Cocktails : doublons des jus.)

### Boissons chaudes
Café noir 8 · Café expresso 8 · Café américain 8 · Café crème 10 · Chocolat chaud 15 ·
Chocolat fondu 25 · Cappuccino 15 · Nescafé 10 · Aromatisé +5 (vanille / caramel / noisette)

### Nos services (non commandable)
Organisation des fêtes, déplacement (anniversaire, fiançailles, ouverture), soutenances,
commandes personnalisées. Instagram @mchou._._

## Décisions (2026-10-14)

- **Une seule source de données** : la vraie carte remplace les données de démo partout
  (Menu, Home, builder, panier, suggestions).
- Jus à deux prix : **Nature / Au lait**.
- Pack choux « 12 pièces brisée 190 DH » : **masqué** tant que le prix n'est pas confirmé.
- Suppléments disponibles sur crêpes (sucrées, spaghetti, spéciales), gaufres, brownies et choux.
- Choux classiques et choux farcis regroupés : une ligne par parfum, la façon (classiques /
  farcis à la crème) est une option, comme le nombre de pièces. La crème (vanille / chocolat)
  n'apparaît que pour les farcis.
- Comme dans le design system : tap sur la ligne = feuille d'options, « + » = ajout direct de
  la variante par défaut (la moins chère).
- Orthographe corrigée sur les intitulés évidents (« Box mixte », « Café américain »,
  « Chocolat chaud »), noms produits conservés tels quels (« pistachio », « pistachou »).
- La feuille d'options sert de fiche produit pour les boissons et les packs ; les douceurs
  ouvrent le builder (décision du 2026-10-14, voir builder.md).

## Ordre des familles (2026-10-16)

Choux, Crêpes, Brownies, Gaufres, Spaghetti, Mojitos, Milkshakes, Cocktails, Jus, Boissons chaudes,
Spéciales, Packs. Même ordre dans les pills du Menu et de Home ; un tap sur une pill de Home ouvre
le Menu à la section de la famille.
