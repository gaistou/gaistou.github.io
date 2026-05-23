# CLAUDE.md — Blog gaistou.github.io

## Contexte du projet

Blog personnel de **Gais**, doctorant à la Chaire Cyber des Systèmes Navals. Le blog est publié à l'adresse **https://gaistou.github.io**, entièrement en **français**, et porte sur la cybersécurité, l'IA et l'informatique — avec un angle particulier sur le Zero Trust et les systèmes OT (Operational Technology).

## Stack technique

- **Jekyll** avec le thème **Chirpy v7.3** (`jekyll-theme-chirpy ~> 7.3`)
- Ruby 3.3, Kramdown + Rouge pour la coloration syntaxique
- Déploiement : **GitHub Pages** via GitHub Actions (`.github/workflows/pages-deploy.yml`)
- Push sur `main` → build Jekyll en production → html-proofer → deploy
- Pas de baseurl, URL directe : `https://gaistou.github.io`

## Structure du dépôt

```
_posts/          # Articles (YYYY-MM-DD-titre.markdown)
_tabs/           # Pages fixes : about, archives, categories, tags
_data/           # authors.yml, contact.yml, share.yml
_plugins/        # posts-lastmod-hook.rb (last_modified_at depuis git)
assets/images/   # Visuels des articles + logo.png (avatar)
tools/           # run.sh (serveur local), test.sh (html-proofer)
.devcontainer/   # Dev container VSCode (jekyll:2-bullseye)
```

## Conventions des articles

- Fichiers : `_posts/YYYY-MM-DD-titre.markdown`
- Front matter minimal :
  ```yaml
  ---
  title: "Titre de l'article"
  date: YYYY-MM-DD HH:MM:SS +0200
  categories: [Catégorie]
  tags: [tag1, tag2]
  ---
  ```
- Les images des articles vont dans `assets/images/`
- `last_modified_at` est injecté automatiquement par le plugin git (ne pas le renseigner manuellement)
- Pas de provider de commentaires configuré (champ `comments` ignoré en pratique)
- Pas d'analytics

## Style éditorial

Influences principales : **LessWrong** (rigueur épistémique, reconstruction depuis les fondations, incertitude calibrée) et **Monsieur Phi** (pédagogie accessible, ton décontracté sur des sujets rigoureux, rendre intuitive une idée abstraite avant d'en donner la formulation précise).

### Ton et voix

- Registre **informel mais rigoureux** : interpellation directe du lecteur, registre décontracté, sans jamais sacrifier la précision technique
- Prise de position franche — les articles ne se contentent pas d'expliquer, ils défendent une thèse souvent provocatrice ou contre-intuitive
- Honnêteté intellectuelle avant tout, y compris sur ses propres limites et incertitudes
- Humour discret, cynisme ponctuel, références subtiles
- L'auteur parle à la première personne, souvent à partir de son expérience personnelle

### Structure type

- **Accroche forte** : le premier paragraphe pose immédiatement le problème de façon provocatrice ou contre-intuitive, sans introduction générique
- **Reconstruction depuis les bases** : les articles ne partent pas des conclusions — ils reconstruisent le raisonnement pas à pas, le lecteur arrive à la conclusion en même temps que l'auteur
- **Exemples concrets**
- **Annexes** pour les démonstrations formelles, afin de ne pas alourdir le corps de l'article
- **Ouverture sur la suite** en conclusion, souvent sous forme de question ou de piste ("Affaire à suivre !")

### Pédagogie technique

- Les formules MathJax sont toujours précédées d'une explication en langage naturel — la formule illustre, elle ne remplace pas
- Les tableaux et graphiques rendent visible ce qui est difficile à intuiter
- Les articles déconstruisent souvent les arguments marketing ou les idées reçues avant de proposer une grille de lecture rigoureuse
- Angle critique systématique : montrer ce qui ne fonctionne pas, et pourquoi, avant de proposer des pistes

### Ce qu'il faut éviter

- Les introductions encyclopédiques ("La cybersécurité est un domaine important…")
- L'accumulation de définitions sans angle critique ni prise de position
- Tout ce qui ressemble à de la communication corporate ou à un article SEO générique
