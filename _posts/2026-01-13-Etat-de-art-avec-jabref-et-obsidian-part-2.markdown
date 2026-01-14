---
layout: post
title: Construire son état de l'art avec JabRef et Obsidian 2/2
tags: [recherche]
---

Dans l’article précédent, j’ai présenté la méthode de travail que j’utilise pour ma thèse, basée sur Obsidian et JabRef. Aujourd'hui nous verrons comment sauvegarder, versionner et partager ce type de travail grâce à Git et Gitea, et en particulier comment adapter les fonctionnalités d'Obsidian (liens, graphe) pour qu'elles soient bien retranscrites dans Gitea.


## Git et Gitea

Git est un système de gestion de versions initialement conçu pour le code source, mais dont les principes s’appliquent très bien à tout fichier textuel, comme les fichiers Markdown ou BibTeX utilisés par Obsidian et JabRef. Git permet de suivre l’évolution d’un ensemble de fichiers dans le temps, de revenir à des états antérieurs, de comparer des versions et de documenter les changements successifs. 

On gère habituellement ses projets Git via une interface web dédiée, la plus connue étant GitHub. Dans mon cas, j’ai choisi d’utiliser Gitea pour des raisons de gratuité et d’auto-hébergement. Je n’ai pas testé cette méthode avec GitHub ou GitLab, et je ne sais donc pas dans quelle mesure la suite de l’article s’y appliquerait telle quelle.

L’ensemble de l’arborescence de fichiers présentée dans l’article précédent constitue ainsi un projet Git, poussé sur Gitea. J’ai ainsi une sauvegarde à jour de mon travail en permanence. Git me permet également d’accéder à mes notes depuis n’importe quel environnement, chez moi, au laboratoire ou en déplacement, et de synchroniser systématiquement les modifications.


## Les limites de Gitea pour un projet Obsidian

Cependant, on rencontre rapidement deux problèmes quand on pousse un projet Obsidian sur Gitea.

Le premier concerne les liens internes. Obsidian utilise une syntaxe de liens spécifique (`[[Note]]`, `[[Note|alias]]`) qui n’est pas compatible avec le Markdown attendu par Gitea. Par défaut, les liens ne sont donc pas résolus côté Gitea et pointent vers des pages inexistantes.

Le second problème concerne le graphe. Le graphe Obsidian est un outil purement interne, généré dynamiquement à partir des liens entre les notes. Obsidian ne fournit pas de mécanisme moyen simple pour l’exporter, ou le re-générer. Spoiler : je n’ai malheureusement pas encore trouvé de solution satisfaisante pour ce point, et le graphe ne sera donc pas traité dans cet article.

Nous allons donc nous concentrer sur la correction des liens dans Gitea.


## La différence entre les liens Obsidian et les liens Gitea

Les liens Obsidian et les liens Markdown utilisés par Gitea n'utilisent pas le même format. Dans Obsidian, un lien comme `[[PDP]` ou `[[Measham2001|Policy Decision Point]]` ne pointe pas vers un chemin de fichier explicite, mais vers une note identifiée uniquement par son nom. C'est Obsidian qui se charge de résoudre le lien, et de retrouver le bon fichier, quel que soit son emplacement réel dans l’arborescence. À l’inverse, Gitea ne comprend que des liens Markdown classiques, avec des chemins relatifs explicites vers des fichiers existants, par exemple : `[@Measham2001](../../../Reading%20notes/@Measham2001.md)`.

On a donc deux problèmes. D’une part, il faut transformer les liens Obsidian en liens Markdown pointant vers le bon chemin relatif dans le dépôt Git. D’autre part, il faut préserver correctement les alias (`[[Note|alias]]`).


## Python à la rescousse

Il nous faut donc un outil capable de traduire les liens Obsidian en liens compatibles avec Gitea. Comme je n’ai pas trouvé de solution existante correspondant exactement à ce besoin, je l’ai fait moi-même* en Python. Vous pouvez retrouver l'ensemble du code sur mon Github [ici](https://github.com/gaistou/Obsidian_to_gitea).


La logique est la suivante :

- altlink.py contient la logique de conversion : parsing des liens Obsidian, gestion des alias (`[[Note|alias]]`), résolution du fichier cible, et génération d’un lien Markdown relatif compatible Gitea.
- export_all.py parcourt le vault Obsidian et génère une copie complète du projet, dans laquelle tous les liens sont réécrits au format Gitea.
- un hook Git (type pre-push) exécute export_all.py automatiquement avant chaque push. La version originale (liens Obsidian) est publiée sur un dépot de sauvegarde et de versionning, la version modifiée (liens Gitea) est publié sur un dépôt de partage. 

Dans mon cas, les deux fichiers Python sont placés dans le dossier "Scripts" de mon projet Obsidian. Le fichier pre-push doit lui être placé dans votre dossier `.git/hooks` à la racine du projet Obsidian.

Le résultat final est donc deux dépôts Git distincts hébergés sur Gitea, chacun avec un rôle différent. Chaque push publie à la fois sur les deux dépôts.

Le premier dépôt correspond à la version brute du vault Obsidian. C’est celui que j’utilise au quotidien pour travailler, sauvegarder et versionner mes notes. C’est également ce dépôt que je pull lorsque je travaille sur plusieurs machines ou environnements.

Le second dépôt sert à la publication. Je ne le pull jamais et je ne travaille pas dessus. Ce dépôt permet uniquement à d’autres personnes de naviguer simplement dans mes notes via Gitea, sans avoir à instlaler Obsidian ou quoi que ce soit. Et l'automatisation fait que ce dépot est toujours synchronisé avec le dépot principal.

![image](/assets/images/obsidian200.png){: width="1000" height="1000"}
*Une note consultable dans Gitea. Tous les liens sont cliquables !*


## En conclusion

Ainsi s'achève cette petite série d'articles sur ma méthode de travail.

Cette démarche de publication de mes notes est particulièrement utile pour partager mon avancement avec mon directeur de thèse et mes encadrants. Je peux facilement me référer à mes travaux en cours et leur envoyer des liens vers les différents concepts que j'explore.

Je n’ai en revanche pas encore trouvé de solution satisfaisante pour exporter le graphe et l’afficher dans Gitea. Le graphe reste donc, pour l’instant, un outil auquel moi seul ai accès. Mais qui sait, ce sera peut-être l’occasion d’un troisième article.

\* J'avoue c'est principalement ChatGPT en fait