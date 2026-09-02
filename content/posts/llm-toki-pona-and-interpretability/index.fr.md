---
title: "Un LLM en toki pona, pourquoi ?"
date: 2026-09-02
draft: false
description: "Une langue complète de 140 mots comme organisme modèle pour l'interprétabilité des LLM : le pari, les difficultés, et ce qu'on peut espérer en tirer."
tags: ["toki pona", "interprétabilité", "LLM"]
categories: ["Interprétabilité"]
---

Est-ce qu'une langue de 140 mots permettrait de mieux comprendre ce qui se passe dans un LLM ? Dans toutes les disciplines qui traitent de systèmes complexes, un modèle *simple* a permis des avancées énormes : *E. coli* et la drosophile en biologie, l'oscillateur harmonique en physique.

En reconnaissance d'image, MNIST joue ce rôle : une tâche **complète**, dix chiffres et rien d'autre. Idéal pour apprendre, comprendre, tester. J'ai fait mes premiers réseaux de neurones sur ce jeu de données, et je mesure avec le recul à quel point un objet simple facilite l'expérimentation.

Dans les LLM, une démarche voisine existe : Tiny Shakespeare et TinyStories, par exemple, où l'on travaille sur un corpus réduit. L'initiative BabyLM est une autre direction intéressante, où l'on réduit le budget de données en se demandant « combien de mots un enfant entend-il avant de parler ? ». Mais dans les trois cas, on reste **à l'intérieur d'une langue immense**, dont les règles viennent d'un corpus infiniment plus large que celui qu'on donne au modèle.

Le toki pona est totalement différent : c'est **une petite langue complète, sans corpus plus vaste derrière elle** — et dans laquelle on peut déjà s'exprimer.

## Les difficultés

La première difficulté, celle qui m'a le plus ralenti, est… la barrière de la langue. Pour la travailler, il faut l'apprendre ! Même si c'est probablement la langue la plus simple à apprendre au monde, c'est une barrière à prendre en compte.

J'avais ce projet en tête en 2024 mais… je n'aime pas apprendre une langue seul ! Ignorant tout ce que l'humanité a appris sur l'adolescence, j'ai proposé à mon fils de l'apprendre avec moi, espérant par ailleurs de beaux moments de complicité père-fils. Il ne m'a fallu que deux années pour me résoudre à l'apprendre seul. À ma décharge, j'avais décelé dix minutes de curiosité dans ses yeux, alors que ma proposition d'apprendre le cunéiforme ensemble n'avait suscité qu'un regard dépité entre deux vidéos TikTok.

Face à cela, j'ai songé à m'orienter vers le **Minimal English** (Goddard & Wierzbicka), une restriction de l'anglais à environ 400 mots, pensée comme une langue véhiculaire simple, ou vers son ancêtre le **Basic English** d'Ogden. Mais au-delà d'une curiosité de longue date pour le toki pona, avoir un **objet linguistique complet** — et pas un sous-ensemble découpé dans une langue plus grande — me semble un véritable terrain d'expérimentation.

Et il y a un risque que je n'arrive pas à écarter : un sous-ensemble d'une langue que je connais déjà n'est pas un système clos. Ses mots gardent le sens qu'ils ont dehors. Le modèle peut en hériter, par son tokenizer ou par un pré-entraînement… Et moi, je le projette en lisant les résultats : je crois comprendre ce que le modèle fait de *dog* parce que je sais déjà ce que *dog* veut dire. En toki pona, *soweli* ne me rappelle rien : il faut expliciter ce qu'il couvre !

Cette inquiétude a d'ailleurs donné naissance au Minimal English. Wierzbicka soutient depuis longtemps que faire de l'anglais la langue par défaut d'une discipline y importe des concepts spécifiquement anglais sans qu'on s'en aperçoive (*Imprisoned in English*, 2014).

Par ailleurs, en tant que Français, une ancienne rivalité me pousserait même à dire que l'anglais n'est peut-être pas la meilleure « lingua franca » :). Le toki pona n'est ni l'anglais ni la lingua franca de personne, ce qui est autant un risque qu'une opportunité intéressante.

## Le pari

La véritable inconnue ? Le pari **d'universalité** : les mécanismes qu'on observera dans un modèle entraîné sur cette langue se transfèrent-ils à des modèles entraînés sur des langues et des corpus incomparablement plus larges ? C'est une version forte de l'hypothèse d'universalité [(Olah et al., 2020)](https://distill.pub/2020/circuits/zoom-in/).

Ce pari existe dans d'autres domaines et n'a pas toujours fonctionné. Quand il échoue, le modèle devient généralement l'objet d'étude. L'histoire des échecs en IA en est un exemple : elle a produit d'excellents programmes de jeu et presque rien sur l'intelligence générale. On peut la résumer en deux citations :

> **Alexander Kronrod** (mathématicien soviétique, ~1965) : *« Chess is the Drosophila of artificial intelligence. »*
>
> **John McCarthy**, *[Chess as the Drosophila of AI](http://jmc.stanford.edu/articles/drosophila/drosophila.pdf)* (in *Computers, Chess, and Cognition*, 1990) : *« It is as if the geneticists after 1910 had organized fruit fly races and concentrated their efforts on breeding fruit flies that could win these races. »*

Le risque que le modèle excelle en toki pona et n'amène aucun résultat significatif sur d'autres sujets est réel. Cependant, l'investissement pour le découvrir me paraît minime et, si c'était le cas, le titre d'« entraîneur de drosophiles de course » serait une consolation parfaitement valable.

## La suite

J'ai un premier modèle entraîné, un corpus et l'envie de répliquer des sujets déjà connus en interprétabilité. Avec plaisir pour en discuter !

Pour en savoir plus sur le toki pona, une langue minimale (moins de 140 mots, 14 phonèmes) inventée par Sonja Lang en 2001, et qui compte une véritable communauté de locutrices et locuteurs : [tokipona.org](https://tokipona.org/).
