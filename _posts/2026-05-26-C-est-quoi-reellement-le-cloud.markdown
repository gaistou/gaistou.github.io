---
layout: post
title: "C'est quoi réellement le cloud ?"
tags: [cloud]
---

Le mot *Cloud* est champion dans sa catégorie de "mot qu'on utilise à tout va sans vraiment savoir ce que ça signifie". La définition *naïve*, la première qu'on apprend c'est : *un serveur qui n'est pas chez moi*.

Mais cette définition est beaucoup trop large, elle inclue en fait tous les serveurs de la planète. Et pourtant elle est aussi trop restrictive, puisque ça exclue tous les SaaS, qui ne sont pas des serveurs, mais des services, et qui sont pourtant considérés comme des *technologies cloud*.

Puis on y réfléchit et on se rend compte que l'usage du mot *cloud* est bizarre. Des fois c'est un nom : *dans le cloud* qui a l'air de définir un truc physique, des fois c'est un adjectif *un service cloud*, un *serveur cloud*, une *architecture cloud*. Et dans les deux cas c'est pas clair, *le cloud* on voit pas vraiment l'objet ou le concept que ça désigne, et l'adjectif *cloud* on sait pas trop ce qu'il qualifie.

A force, on finit par être agacé à chaque utilisation du mot cloud. Les conversations sont pas claires, demandent des efforts d'interprétation et d'adaptation à ce qu'entend l'interlocuteur par *cloud*, ça crée des quiproquos, et à la fin on sait plus vraiment ce qu'on vient d'acheter.

C'est malheureusement l'usage du mot *cloud* au quotidien, et ça on ne pourra plus rien y faire. Mais dans un effort d'intégrité intellectuelle, je vous propose d'essayer de redonner une définition claire au mot *cloud*.


## Un peu de rigueur ontologique

Commençons par la définition de référence, celle du NIST.

> *"Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g., networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction."*

La première chose à remarquer, c'est que le cloud c'est un **modèle de prestation de mise à disposition de ressources informatiques**. Ce n'est pas une technologie, un produit, ou un lieu physique. C'est une façon de qualifier une relation commerciale entre un fournisseur et un consommateur.

Un logiciel ne peut pas être *cloud*. Un serveur ne peut pas être *cloud*. Un réseau ne peut pas être *cloud*. Rien n'est situé *le cloud*. On ne fait pas de sécurité *du cloud*. Ce ne sont pas les bonnes catégories ontologiques. Ce qui est *cloud*, c'est la **prestation** qui vous donne accès à ces ressources.

Dit autrement : « AWS S3 c'est du cloud » est un raccourci pour « la prestation par laquelle AWS vous fournit du stockage satisfait les critères d'une prestation cloud ». Le service de stockage lui-même n'est ni cloud ni non-cloud, c'est juste du stockage. Le moyen technique par lequel AWS rend possible cette prestation **n'est pas un critère**.

Le mot *cloud* est par nature un terme marchand, qui définit un type de prestation commerciale. Il n'apporte rien dans une discussion technique, dans laquelle il faudra préférer d'autres termes : calcul distribué, répartition de charge, délégation d'authentification, serveur de calcul, télémétrie, etc ...


## La nature d'une prestation cloud

Revenons à l'essentiel : qu'est-ce qu'un fournisseur cloud peut vous livrer ?

**Des ordinateurs** ou des fractions d'ordinateurs : du CPU, de la RAM, du stockage, du réseau. C'est ce qu'on appelle l'**IaaS** (*Infrastructure as a Service*).

**Des logiciels** servis sur un réseau, que vous utilisez sans vous préoccuper de l'infrastructure sous-jacente. C'est le **SaaS** (*Software as a Service*). J'en parle plus en détail dans [cet article](https://gaistou.github.io/posts/Le-saas-est-un-objet-marketing/).

La distinction fondamentale entre les deux, c'est la **responsabilité** : avec l'IaaS, le fournisseur vous livre une machine vide — ce qui tourne dessus est votre affaire. Avec le SaaS, le fournisseur garde la responsabilité du logiciel, vous n'accédez qu'à sa surface.

Ces deux catégories sont exhaustives parce que l'informatique, ce n'est rien d'autre que du matériel et des logiciels qui tournent dessus. Un fournisseur cloud ne peut vous livrer que l'un ou l'autre, ou les deux, à des niveaux d'abstraction différents.


## T'as oublié les PaaS, FaaS, MLaaS, CaaS, Baas, DBaaS, DaaS, EaaS, GaaS, HaaS, JaaS ...

> On a fini de reciter ... Les vingt-six lettres de l'alphabet.

Tout ça ne sont que des sous catégories de IaaS ou SaaS. On peut se faire un petit exercice pour s'en convaincre :

- **PaaS** (Heroku, Railway, Google App Engine) : vous déposez du code, vous recevez un environnement d'exécution pré-configuré. C'est de l'IaaS avec une config particulière.
- **CaaS** (Google Kubernetes Engine, Amazon ECS) : vous recevez une infrastructure d'orchestration de conteneurs. C'est de l'IaaS.
- **DaaS** (Amazon WorkSpaces, Azure Virtual Desktop) : vous recevez un bureau virtuel, c'est-à-dire un ordinateur. C'est de l'IaaS.
- **FaaS** (AWS Lambda, Google Cloud Functions) : vous déposez une fonction, un service logiciel l'exécute pour vous. C'est du SaaS.
- **DBaaS** (AWS RDS, MongoDB Atlas, PlanetScale) : vous recevez une base de données gérée, c'est un logiciel servi sur un réseau. C'est du SaaS.
- **MLaaS** (OpenAI API, Google Vertex AI, AWS SageMaker) : vous consommez un service d'inférence ou d'entraînement de modèles. C'est du SaaS.
- etc ...


## Résultat : ma définition formelle du cloud

Le cloud, c'est une prestation de livraison de ressources informatiques. Ces ressources sont soit du matériel (IaaS), soit du logiciel (SaaS). La prestation doit remplir 4 critères :
- **On-demand self-service** : vous déclenchez la prestation vous-même, sans appeler quelqu'un chez le fournisseur
- **Broad network access** : la ressource est accessible sur un réseau via des protocoles standards (HTTP, SSH...)
- **Resource pooling / Rapid elasticity** : les ressources s'adaptent à votre consommation, vous scalez à la demande
- **Measured service** : l'usage est mesuré, vous payez ce que vous consommez

Avoir une définition précise du cloud, ce n'est pas juste de l'hygiène intellectuelle, ça permet de se poser les bonnes questions.

*"Passer au cloud"* n'est pas une décision technique, c'est une décision sur la **responsabilité**. C'est le choix de déporter une partie de sa responsabilité à un tiers via une prestation commerciale. La scalabilité, la redondance, la facturation à l'usage ... ce ne sont en fait que les conditions naturelles qui rendent possible ce genre de prestation, ce ne sont pas des finalités.