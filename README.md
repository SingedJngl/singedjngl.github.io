# singed.github.io — journal de projets

Site Jekyll bilingue (FR / EN), sans plugin : il se construit tel quel avec le
moteur natif de GitHub Pages.

## Mise en route

```bash
bundle install
bundle exec jekyll serve   # http://localhost:4000
```

## Déploiement

1. Nommer le dépôt `singed.github.io` (obligatoire pour un site utilisateur).
2. Pousser sur la branche `main`.
3. Settings → Pages → Source : *Deploy from a branch*, `main` / `(root)`.

## Comment fonctionne le bilinguisme

Il n'y a pas de plugin de traduction. Le mécanisme tient en trois règles :

| Élément | Français | Anglais |
|---|---|---|
| Accueil | `index.html` → `/` | `en/index.html` → `/en/` |
| Articles | `_posts/fr/` → `/journal/...` | `_posts/en/` → `/en/log/...` |
| Pages | `a-propos.md` | `en/about.md` |
| Textes d'interface | `_data/i18n.yml`, clé `fr` | `_data/i18n.yml`, clé `en` |

Deux articles qui sont la traduction l'un de l'autre partagent la **même clé
`ref:`** dans leur front matter. C'est ce qui permet au sélecteur de langue de
renvoyer vers l'article correspondant plutôt que vers l'accueil. Pour les pages
fixes, on utilise `alt_url:` à la place.

## Ajouter un article

Créer deux fichiers, un par langue, avec la **même date** et le **même `ref`** :

```
_posts/fr/2026-10-02-mon-projet.md
_posts/en/2026-10-02-my-project.md
```

Front matter type :

```yaml
---
layout: post
title: "Titre de l'article"
ref: mon-projet          # identique dans les deux langues
lang: fr                 # fr ou en
permalink: /journal/mon-projet/
status: "En cours"       # facultatif, affiche un badge
cover: /assets/img/mon-projet.jpg
cover_alt: "Description de l'image pour les lecteurs d'écran"
cover_caption: "Légende sous l'image"     # facultatif
excerpt_text: "Deux ou trois phrases. C'est ce qui s'affiche sur l'accueil."
stack: ["ESP32", "C++"]  # facultatif, affiché en pied d'article
---
```

`excerpt_text` est un champ à remplir à la main plutôt que l'extrait automatique
de Jekyll : sur une page d'accueil aussi dépouillée, ces deux phrases font la
moitié du travail.

## Images de garde

Format conseillé : **1200 × 675 px** (16:9), JPEG, dans `assets/img/`.
Les trois fichiers `.svg` actuellement en place sont des gabarits temporaires —
à remplacer par de vraies photos.

Un conseil pour les photos : plan serré sur la carte ou le montage, lumière
diffuse, fond neutre. Une photo d'établi honnête vaut mieux qu'un rendu 3D.

## Ce qui reste à faire

- [ ] Remplacer les trois images de garde
- [ ] Compléter les `<!-- TODO -->` dans les articles (mesures, courbes, schémas)
- [ ] Déposer le CV PDF dans `assets/cv/` et le lier depuis « À propos »
- [ ] Vérifier le rendu sur mobile après ajout des vraies photos
