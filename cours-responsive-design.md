---
tags:
  - btech
  - session-48
  - course
---

# Jour 11 : Responsive Design

---

Le **reponsive design** consiste à créer des pages qui :

- s'adaptent à la largeur de l'écran
- restent lisibles sur mobile
- évitent les scrolls horizontaux
- réorganisent l'information selon l'espace disponible

> ℹ️ Quelle utilité à faire du responsive ?
>
> - 80% du traffic web sur portable ("mobile-first"),
> - toujours une importance sur le desktop coté entreprises, donc à ne pas tout faire sur téléphone non plus !

---

## Layouts selon l'écran

Selon le type d'écran, l'**organisation de la page** ("_layout_") ne sera généralement pas la même :

- **Desktop** :
  - plusieurs colonnes possibles,
  - plusieurs éléments côte-à-côte possible,
  - tailles des zones cliquables selon l'importance.

- **Mobile** :
  - une seule colonne,
  - un seul élément par ligne,
  - tailles des zones cliquables larges.

---

Concrètement, sur **mobile** :

- une `width` proche de 100%,
- rarement du `margin` près des bords de l'écran,
- moins de `padding` car moins de place,
- idem pour `font-size`,
- le contenu au **centre** car on lit de haut en bas, pas de gauche à droit.

---

## Media Queries

Pour adapter un layout d'un écran à un autre, il faut d'abord connaître leurs dimensions.

On utilise pour cela une règle CSS "at-rules", `@media`, pour donner des instructions au sein d'un fichier CSS (similaire à `@keyframes`).

---

### `@media`

C'est ce qui va permettre de **s'adapter à l'appareil utilisé** et ses **caractéristiques** :

```css
@media <paramètres> {
  /* règles CSS ici */
}
```

---

### Types de l'appareil

Quel type d'appareil veut-on cibler ?

- `all` : tous les appareils
- `print` : les impressions
- `screen` : tous les écrans
- `speech` : les narrateurs d'écran

> ℹ️ On utilisera principalement `screen`. Les autres types d'écran (anciennement "braille", "tv", etc...) sont aujourd'hui incluent dans les 4 types ci-dessus ("braille" → "speech" ; "tv" → "screen" ; ...).

```css
@media screen {
  body {
    background-color: red;
  }
}
```

---

### Propriétés de l'appareil

On peut tester les propriétés d'un appareil, comme :

- pour un **écran** :
  - sa `height` ou sa `width`,
  - son `orientation`,
  - s'il utilise un `pointer`,
  - thèmes, accessibilité, ...

- pour un **narrateur** :
  - sa langue,
  - une navigation au clavier ou non,
  - des médias à lire (photos, vidéos, ...).

Et beaucoup d'autres !

Par exemple, si on souhaite n'appliquer un changement que lorsqu'un appareil est en mode "portrait" :

```css
@media (orientation: portrait) {
  div {
    width: 100%;
    margin: 0px;
    text-align: center;
  }
}
```

> ⚠️ Quelques points d'attention :
>
> - il est vivement conseillé d'indiquer le type de média car tous n'ont pas accès aux mêmes propriétés,
> - grâce aux opérateurs logiques d'un media-query (partie du cours suivante), on peut vérifier plusieurs propriétés en simultané.

---

### Opérateurs logiques

On peut combiner **plusieurs paramètres** dans une même _media-query_ :

- `and` : "et si..."
- `not` : "si n'est pas..."
- `only` : "seulement si..."
- `,` ("_or_") : "ou si..."

Par exemple, on vérifie d'abord que l'on se trouve bien sur un appareil avec écran avant de vérifier si celui-ci est en mode "portrait" :

```css
@media only screen and (orientation: portrait) {
  div {
    width: 100%;
    margin: 0px;
    text-align: center;
  }
}
```

> ℹ️ Concernant `only` :
>
> - appliquera la nouvelle mise en forme **_uniquement_** si toute la media-query est valide,
> - empêche les anciens navigateurs d'en appliquer seulement la première partie - de la media query,
> - exemple : anciennement, avec `@media screen and (orientation: portrait)`, les anciens browsers s'arrêterait à `screen` et skiperait le reste (`and (orientation: portrait)`) !
> - il est donc important de toujours indiquer `only` entre `@media` et le type de média.

---

## Aparté : `viewport` et responsive design

Pour forcer le (dé)zoom et manipuler les dimensions d'une page web, il faudra toujours ajouter ceci dans la balise `<head>` :

```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
</head>
```

---

## Breakpoints (écrans)

Pour adapter le layout plus précisément, on va chercher à connaître la **largeur** de l'écran plutôt que son orientation :

```css
@media only screen and (max-width: 425px) {
  div {
    width: 100%;
    margin: 0px;
    text-align: center;
  }
}
```

> ℹ️ `min-` et `max-` :
>
> - par défaut en CSS, `min-` et `max-` attribuent un minimum ou maximum à une propriété ; par exemple `min-height: 500px` donne au minimum 500 pixels de hauteur à un élément ;
> - ici, avec les medias queries, on vérifie si on utilise un mobile, et pas plus grand, en comparant la taille de l'écran à un maximum de 425 pixels en largeur,
> - conseil : privilégiez ce type de media-query plutôt que les exemples précédents, car c'est le plus simple et le plus courant.

Il existe aujourd'hui _beaucoup_ d'écrans de tailles différentes.

On se base alors sur des **standards** :

| Largeur       | Appareils                            |
| ------------- | ------------------------------------ |
| 320px-425px   | mobiles                              |
| 426px-768px   | tablettes                            |
| 769px-1024px  | ordinateurs portables, petits écrans |
| 1025px-1440px | ordinateurs fixe, grands écrans      |

> ℹ️ On parle de _standards_ : il existe toujours des **exceptions** !
