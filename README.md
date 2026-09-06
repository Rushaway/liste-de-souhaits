# Nos listes de souhaits

Site statique (HTML/CSS/JS inline, aucune dépendance, aucun build). Deux listes
de cadeaux hébergées sur le même repo. Mobile-friendly.

| Page | URL |
|------|-----|
| Accueil (choix de la liste) | https://rushaway.github.io/liste-de-souhaits/ |
| Liste de Nicolas | https://rushaway.github.io/liste-de-souhaits/nicolas/ |
| Liste de Lucile | https://rushaway.github.io/liste-de-souhaits/lucile/ |

## Structure

```
index.html          → page d'accueil, liens vers les deux listes
nicolas/index.html  → liste de Nicolas
lucile/index.html   → liste de Lucile
```

## Aperçu en local

Ouvrir le `index.html` voulu dans un navigateur. C'est tout.

## Déploiement (CI/CD → GitHub Pages)

Le workflow [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)
publie le site sur GitHub Pages à **chaque push sur `main`** :

1. `actions/upload-pages-artifact` empaquette la racine du repo ;
2. `actions/deploy-pages` met le site en ligne.

### Activer Pages (une seule fois)

Dans **Settings → Pages → Build and deployment → Source**, choisir
**GitHub Actions**. En ligne de commande :

```bash
gh api repos/{owner}/{repo}/pages -X POST -f build_type=workflow
```

### Récupérer l'URL du site

```bash
gh api repos/{owner}/{repo}/pages --jq .html_url
```

## Ajouter un cadeau

Ajouter un objet dans le tableau `gifts` de `nicolas/index.html` ou
`lucile/index.html` :

```js
{ id: 'slug-unique', title: 'Nom du cadeau', note: 'Pourquoi ça me plaît.',
  price: '≈ 30 €', url: 'https://…', icon: 'gift',
  added: '2026-09-06',      // date d'ajout (AAAA-MM-JJ) — affichée « Ajouté le 6 septembre 2026 »
  badge: 'coup de cœur' }   // optionnel — bandeau rouge (coup de cœur, taille XL, petit prix, à confirmer…)
```

Champs : `id`, `title`, `note`, `price` obligatoires ; `url` (sinon bouton
désactivé), `icon`, `added`, `badge` optionnels ; `example: true` pour une
carte de démo (lien grisé « Lien à venir »).

Les listes servent pour Noël **et** les anniversaires — les citations en haut
de page (tableau `JOKES`) tournent au hasard à chaque chargement, ajoutes-en
si l'envie te prend.

Les icônes disponibles sont listées dans l'objet `ICONS` en haut du
`<script>`. Ajouter une nouvelle icône SVG plutôt que d'en réutiliser une qui
ne correspond pas. Un `push` sur `main` suffit à mettre le site à jour.
