---
layout: post
title: Le SaaS est un objet marketing
categories: [Cloud]
tags: [cloud]
---

## Tout est un SaaS, rien est un SaaS

Le NIST (National Institute of Standards and Technology) définit le SaaS comme suit :

> *"The capability provided to the consumer is to use the provider's applications running on ... blablabla"*

Je vous épargne la formule pompeuse, dans un langage clair ça donne les critères suivants :

1. Logiciel hébergé sur une architecture *cloud*.
2. Accès réseau via des protocoles standards (HTTP typiquement).
3. Le client ne gère pas l'infrastructure.
4. Le même service est fourni à plusieurs clients (multi-tenant).

C'est chouette ça nous fait une définition, mais ça ne correspond pas du tout à l'usage courant du mot SaaS.

Parce qu'avec ces critères, Google Maps est un SaaS. Wikipedia est un SaaS. Le site de votre mairie est techniquement un SaaS. Et pourtant, ni Wikipedia, ni votre mairie ne se vante d'être fournisseur de service cloud.


## Les gens s'en foutent de la définition du NIST

Si on observe comment le mot est utilisé dans la pratique, on retrouve souvent les critères suivants :

- Modèle d'abonnement : on paye pour y avoir accès, souvent par utilisateur ou par usage.
- Utilisateurs professionnels : Salesforce, Slack, Office 365, mais pas Pastebin.
- Remplacement d'un client lourd : Overleaf est un SaaS qui remplace TexMaker.
- Le service est le produit : sur Airbnb, le produit, c'est le logement. Le site n'est qu'un intermédiaire. Sur Salesforce, le logiciel c'est le produit.

Aucun de ces critères n'est technique. Ce sont uniquement des critères marketing. Et tous ces critères sont arbitraires, aucune norme technique ne les a imposés.


## Le SaaS est un objet marketing

Après plusieurs heures à essayer de rationnaliser ce qu'on veut bien dire par SaaS, j'arrive à la conclusion qu'il ne faut pas le rationnaliser techniquement. Ce n'est pas un objet technique. C'est une catégorie commerciale.

Ce qui a fini de me convaincre c'est le subreddit [r/SaaS](https://www.reddit.com/r/SaaS/). Ce n'est pas un subreddit d'ingénieurs qui débattent d'architecture distribuée. C'est un subreddit d'entrepreneurs et de commerciaux. Les discussions portent sur le pricing, sur la rentabilité, sur comment trouver ses premiers clients.

Essayer de rationaliser techniquement c'est prendre le problème à contre-sens. Le mot SaaS ne découle pas d'une analyse rigoureuse des architectures logicielles. Il a été choisi parce qu'il s'alignait bien avec le discours cloud qui est porteur économiquement.


## La sécurité des SaaS

Si le SaaS est une catégorie marketing, alors parler de "sécurité SaaS" est une imprécision que vous devriez refuser. Pas parce que la sécurité de tout ce qu'on appelle des SaaS n'est pas un sujet, évidemment. Mais parce que quand on entre dans le domaine technique (celui de la sécurité), il y a toujours une meilleure façon d'en parler. Ce qui mérite d'être sécurisé, ce sont : les identités, les autorisations, les données, les protocoles, le déploiement, etc ...

Et ce ne sont pas des sujets qui concernent uniquement *les SaaS*, mais le développement logiciel en général en fait. Conceptualiser la sécurité *SaaS* comme un domaine complétement différent, c'est créer du flou là ou il n'y a pas lieu d'en avoir.

Vous ne faites pas de la *cloud security* quand vous configurez l'authentification d'une application web. Arrêtez de vous mentir à vous même.
