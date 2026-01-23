# Blog Raphaël Bellec

Blog personnel Hugo déployé sur GitHub Pages.

## Stack

- **Générateur** : Hugo
- **Thème** : Blowfish (https://blowfish.page)
- **Hébergement** : GitHub Pages
- **Déploiement** : GitHub Actions (automatique sur push)

## URLs

- **Production** : https://rbellec.github.io/blog/
- **Repo** : https://github.com/rbellec/blog

## Structure

```
config/
  _default/           # Config production
    hugo.toml         # Config principale
    params.toml       # Paramètres du thème
    languages.fr.toml # Config langue française
    languages.en.toml # Config langue anglaise
    menus.fr.toml     # Menu français
    menus.en.toml     # Menu anglais
    markup.toml       # Config markdown
  development/
    hugo.toml         # Override baseURL pour dev local
content/
  _index.fr.md        # Page d'accueil FR
  _index.en.md        # Page d'accueil EN
  posts/              # Articles du blog
```

## Multilingue

- **Langue par défaut** : Français (fr)
- **Seconde langue** : Anglais (en)
- Convention de nommage : `index.fr.md` et `index.en.md` dans le même dossier

## Créer un article

```bash
mkdir -p content/posts/mon-article
```

Puis créer `index.fr.md` :
```yaml
---
title: "Titre de l'article"
date: 2025-01-19
draft: false
description: "Description courte"
tags: ["tag1", "tag2"]
categories: ["Catégorie"]
---

Contenu de l'article...
```

Et `index.en.md` pour la version anglaise.

## Commandes

```bash
# Développement local
hugo server

# Build production
hugo --gc --minify

# Le site local est sur http://localhost:1313/
```

## Choix de design

- Pas de fonds dynamiques/animés
- Layout "basic" pour le header
- Layout "profile" pour la homepage
- Apparence claire par défaut (avec switch dark/light)
