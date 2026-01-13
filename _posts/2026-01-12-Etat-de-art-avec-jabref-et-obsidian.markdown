---
layout: post
title: Construire son état de l'art avec JabRef et Obsidian
tags: [recherche]
---


Je me suis récemment engagé dans une thèse de doctorat, ce qui m'a enfin forcé à mettre en place une méthode de travail rigoureuse pour garder un fil dans mes idées et ancrer durablement ce que j'apprends. Après environ quatre mois d’usage quotidien, j’ai fini par stabiliser une méthode et une suite d'outils qui me conviennent et qui correspondent à ma manière de concevoir le travail de recherche, et je souhaite la partager avec vous.

L'idée centrale est la suivante : la recherche ce n'est pas une attente passive de l'illumination, mais un travail itératif de construction et de mise en relation d'idées et de concepts. Et on va construire un système autour de cette idée.

Pour donner une idée concrète du résultat et vous donner envie de lire la suite, voici à quoi ressemble mon environnement de travail après ces quatre premiers mois de recherche.

![image](/assets/images/obsidian1.png){: width="1000" height="1000"}


## Mon outil de prise de notes : Obsidian

Obsidian est un outil de prise de notes basé sur des fichiers Markdown. Chaque note est un simple fichier texte, lisible sans Obsidian, versionnable, stocké localement, indépendant de toute connexion Internet. Le format Markdown permet une prise de notes légère et facile à partager, sans soucis de mise en page ou de format de fichier propriétaire.

Mais le format Markdown n’est pas le principal intérêt d’Obsidian. Son point fort réside dans la possibilité de créer facilement des liens explicites entre les notes, et de visualiser ces liens sous forme de graphe. Chaque note peut être reliée à d’autres par des liens hypertextes, ce qui permet de suivre un raisonnement ou une idée de note en note, comme sur un wiki en somme.

![image](/assets/images/obsidian4.png){: width="1000" height="1000"}
*Un exemple de note dans Obsidian*

Enfin, Obsidian permet également d’organiser les fichiers dans une hiérarchie classique de dossiers. On dispose ainsi de deux modes de navigation complémentaires :
- une navigation hiérarchique, lorsqu’on cherche une note précise ;
- une navigation par liens et par graphe, lorsqu’on souhaite explorer une idée, un concept ou un ensemble de relations.

Pour ma part, voici la hiérarchie de dossiers que j'utilise :
- Archive : Contient des notes obsolètes, des idées abandonnées, des vieux brouillons. Je ne supprime jamais rien ; ce dossier sert de mémoire froide.
- Concepts : Contient les notes conceptuelles et techniques, c'est ce dossier qui sera la source du graphe.
- Images : Contient les images que j'insère dans les autres notes, c'est comme ça qu'Obsidian gère les images.
- Institutions : Contient des notes sur les acteurs de mon domaine de recherche, les chercheurs, les centres de recherche, les organisations, les entreprises, etc.
- Publications : Contient les PDF de toutes les publications que je lis, c'est ce dossier qui sera géré par JabRef.
- Reading Notes : Contient mes notes de lecture associées aux publications. Un plugin Obsidian permettra de faire le lien direct entre une note de lecture et l’article correspondant.
- Rédaction : Contient les parties rédigées de la thèse ainsi que des embryons d’articles destinés à être publiés.
- Scripts : Contient des scripts d'automatisation, en particulier ce dossier sera utilisé par Git pour le partage de certaines notes.
- Vrac : Contient tout ce qui n'est pas assez clair ou abouti pour être dans un des autres dossiers, c'est le bac à sable.

![image](/assets/images/obsidian5.png){: width="210" height="750" }

Le cœur de la structure est le dossier "Concepts", c'est ici que se développent les idées selon la méthode Zettelkasten. Les autres dossiers ne servent que d'espace de travail pour soutenir cet objectif. 


## Prendre des notes efficacement en s'inspirant de Zettelkasten

La méthode Zettelkasten est une approche de prise de notes et de construction de connaissances qui repose sur l’idée que la recherche et la production intellectuelle consistent à faire émerger progressivement des structures de sens à partir d’idées élémentaires. On retrouve ici mon idée de départ.

Concrètement, la Zettelkasten s’appuie sur des notes courtes et atomiques, chacune centrée sur une idée unique, et reliées entre elles par des liens sémantiques. Le réseau ainsi formé permet de naviguer d’idée en idée, de faire apparaître des relations inattendues, et d’identifier des zones encore peu explorées. On retrouve ici des principes proches de ceux mis en avant par Obsidian.

L’un des points clés de cette méthode est la séparation entre les sources (articles, livres, documents) et les notes conceptuelles. Ces dernières constituent un espace de pensée autonome et sont systématiquement rédigées manuellement. Ce détachement de la source force à réinterpréter les concepts à sa manière et à formuler explicitement les liens sémantiques avec les autres concepts. Dans mes notes, je ne mentionne la source que quand c'est réellement important pour comprendre le concept. On retrouve ici une idée que j'aime bien : qu’un concept n’est réellement assimilé que lorsqu’on est capable de se l’expliquer intégralement soi-même.

![image](/assets/images/obsidian7.png){: width="1000" height="1000"}


## Gérer ses sources avec JabRef

JabRef est un gestionnaire de références bibliographiques orienté recherche académique. Il permet de centraliser l’ensemble des publications lues et de leur associer des métadonnées (auteurs, titre, année, DOI, etc.). Toutes les données sont sauvegardées au format BibTeX, qui n’est au fond qu’un fichier texte. Ma bibliothèque JabRef est donc facilement versionnable avec Git.

JabRef prend en charge différents types de sources et permet notamment de récupérer automatiquement les métadonnées d’articles scientifiques à partir de leur DOI. Il est également possible de tagger les références, de leur attribuer une priorité, de les marquer comme lues ou non lues, ou encore d’y associer les fichiers PDF correspondants. En bref, ce n'est rien d'incroyable hormis une boîte à outils efficace pour gérer une bibliothèque numérique.

En pratique, lorsque je lis un nouvel article intéressant :
- j’ajoute une nouvelle entrée dans JabRef ; pour un article scientifique, je renseigne simplement le DOI, sinon je complète les métadonnées de base à la main (titre, auteurs, année) ;
- j’enregistre le fichier PDF correspondant dans mon dossier _Publications_ ;
- je relie le PDF à l’entrée nouvellement créée dans JabRef.

Le dossier _Publications_ est volontairement un grand fourre-tout de fichiers PDF souvent mal nommés. Je ne l’ouvre jamais manuellement. Toutes les interactions avec les sources passent par JabRef, qui me permet de retrouver facilement une référence et d’ouvrir directement le PDF associé.

![image](/assets/images/obsidian8.png){: width="1000" height="1000"}


## Manipuler les sources dans Obsidian

Une grande force d’Obsidian est sa facilité de personnalisation grâce à une large collection de plugins développés par la communauté. Dans mon cas, j’utilise le plugin *Obsidian Citations*, développé par Jon Gauthier. Ce plugin permet à Obsidian de lire directement le fichier BibTeX généré par JabRef, et donc d’interagir avec la bibliothèque de références.

- Créer ou ouvrir une note de lecture sur une source (Ctrl + Shift + O). Si la note existe déjà, la commande ouvre simplement la note correspondante.
- Insérer un lien vers la note de lecture d'une source (Ctrl + Shift + E).

Le plugin permet également de créer un template pour les notes de lecture. Dans mon cas j'ai choisi le template suivant, qui est automatiquement appliqué à la création d'une nouvelle note de lecture.

![image](/assets/images/obsidian9.png){: width="1000" height="1000"}


Dans ces notes de lecture, je relie systématiquement l’article aux concepts qu’il mobilise ou qu’il éclaire. En revanche, je ne développe jamais les concepts eux-mêmes à cet endroit : ils sont traités exclusivement dans le dossier _Concepts_. La note de lecture sert à analyser une source ; elle n’est pas un espace de conceptualisation.

Enfin, une fois la lecture terminée, je reporte la section _pertinence pour ma thèse_ directement dans JabRef, d’un score de pertinence de 1 à 3 (le petit drapeau). Cela me permet d’avoir, côté bibliographie, une vue rapide de l’intérêt d’une source.

## Visualiser les concepts avec un graphe

Une des récompenses après tous ces efforts de prise de note, c'est de pouvoir visualiser tous ses concepts sous la forme d'un graphe généré par Obsidian.

![image](/assets/images/obsidian10.png){: width="1000" height="1000"}

Sur mon graphe, on retrouve :
- les *concepts* en jaune ;
- les *publications* importantes en rouge ;
- les *institutions* en bleu ;
- les *projets* en vert (des outils, des programmes, des implémentations ... qui sont en fait des *concepts* particuliers).

Évidemment, le résultat est fouillis, et les zones denses sont difficilement lisibles. Mais l’intérêt principal n’est pas la lisibilité locale : c’est la vue macro qu’offre le graphe sur l’état de l’art.

Sur ce graphe, je vois clairement émerger les gros nœuds jaunes correspondant à mes concepts centraux : _Zero Trust_, _authentification_, _autorisation_. Je distingue également un nœud rouge principal, qui correspond à la publication de référence autour de laquelle s’articulent de nombreux autres travaux. En périphérie apparaissent des clusters plus petits, liés à des domaines connexes mais secondaires dans le cadre de ma thèse : systèmes industriels, supervision, pentesting. En dehors de la masse, je vois les *concepts*, les *publications* et les *institutions* qui sont peu connectés au reste du graphe, et qui ont peut être finalement peu d'intérêt pour ma thèse.

Il est important de garder à l’esprit que ce graphe n’a rien de magique. Il ne découvre rien par lui-même : il ne fait que refléter les liens que j’ai explicitement posés. Il est **fortement** biaisé par ce que je choisis de lire, de noter, de relier. Lorsque je rédige mes concepts, je réfléchis souvent à comment telle ou telle liaison va apparaître sur mon graphe, ce qui m'incite parfois à volontairement ne pas créer de lien. Mais ce n'est pas grave, le graphe n'est pas un outil formel, c'est juste un outil d'exploration.

En fait, je pense que plus que pour son apport pratique, le graphe est avant tout une récompense satisfaisante qui me motive à continuer d'explorer davantage de sujets.

## Le système en action

Maintenant que j'ai tous mes éléments, je me retrouve avec la boucle de travail suivante :
- Je trouve un nouvel article intéressant.
- Je crée une entrée sur Jabref et je la relie au document.
- Dans Obsidian, Ctrl + Shift + O pour créer une nouvelle note de lecture sur l'article avec le template.
- Au fil de la lecture, j'enrichis mes notes *Concepts* chaque fois que j'apprends quelque chose de nouveau.
- Dans la note de lecture, je liste les *concepts* utilisés dans le papier, et je complète le template.
- Lorsque j'ai un embryon d'idée nouvelle, je la note dans mon dossier *Vrac*.
- A la fin, j'évalue la pertinence de l'article pour ma thèse, et je reporte l'évaluation dans JabRef.

Régulièrement, je reviens sur mes notes conceptuelles, mon graphe et mon dossier _Vrac_ afin d’identifier les idées à consolider, les zones sous-explorées et les pistes qui méritent d’être approfondies. Cette ré-exploration alimente ensuite de nouvelles recherches bibliographiques, et la boucle recommence.

Le dossier *Vrac* est particulièrement important dans ma boucle de travail, c'est souvent ce dossier qui fait l'intermédiaire entre mes *concepts* et mon dossier *Rédaction*. J'y note toutes mes idées, mes réflexions personnelles sur tout ce que je lis. Puis progressivement je les enrichis jusqu'à ce que ça devienne des notes de *concepts* ou des éléments de *rédaction*.

## Conclusion

Voilà la méthode que j’utilise aujourd’hui. Elle me convient, elle me permet d’avancer, et surtout elle correspond à ma manière de concevoir le travail de recherche. Elle continuera très probablement à évoluer au fil de la thèse. En revanche, je pense que son cœur, la séparation des sources et des concepts, les notes atomiques, les liens explicites, restera relativement stable.

Ce système ne vise pas à optimiser la production, ni à garantir la qualité des idées. Il sert avant tout à alléger la charge cognitive du travail de recherche : garder une trace des concepts, rendre visibles les liens, et permettre des allers-retours constants entre lectures, compréhension et reformulation. Il amplifie autant les bonnes intuitions que les mauvaises, et ne remplace en aucun cas la rigueur intellectuelle du bon chercheur.

Un point important n’a volontairement été qu’effleuré ici : la question de la sauvegarde et du partage de ces notes. Ce sera l’objet d’un prochain article, centré sur l’usage de Gitea pour versionner et partager ce type de base de connaissances.