---
layout: post
title: La hiérarchie opérationnelle des métiers de la cyber
tags: [cyber]
---

Ces dernières années, la cybersécurité s’est fragmentée en une multitude de métiers. Les rôles se sont spécialisés, les postes se sont normalisés, chacun a obtenu un titre, une fiche de poste, et une place bien définie dans la grande machine cyber. Résultat : on a construit un écosystème dense et structuré … mais parfois compliqué à comprendre. Même de l’intérieur.

Je propose ci-dessous un modèle hiérarchique des métiers de la cyber, non pas en termes de statut ou d’importance, mais en terme de proximité avec les opérations. Le point de départ est simple, il n'y a qu'un seul acteur libre dans l'écosystème : c'est l'attaquant. C'est lui qui justifie tout le reste de la chaîne. Sans attaque, il n'y a pas d'industrie de la cyber.

A partir de l'attaquant, on peut établir une chaine hiérarchique opérationnelle, dans laquelle chaque étage est justifié par l'étage directement au dessus :
- Pas d’attaque → pas d'incident.
- Pas d'incident → pas de détection.

En dessous de cette chaîne opérationnelle, on peut ajouter deux couches non réactives : la prévention et la gouvernance. Contrairement à ce qu’on pourrait croire, elles ne fondent pas l’opérationnel : c’est l’inverse. Elles n’ont de sens qu’en réaction à l’existence d’attaques, d’incidents et d’équipes qui y répondent.

![](/assets/images/hierarchie_cyber.png)


## Niveau -1 : Conformité & Gouvernance

**Exemples :** GRC, DPO, responsables conformité, RSSI purement administratifs, CTI stratégique

Ici, on retrouve tous les rôles qui gravitent autour de la sécurité sans jamais entrer en contact avec une attaque. Leur production est surtout documentaire : politiques, matrices de risques, schémas organisationnels ... Bref, ils posent le cadre administratif et réglementaire de la sécurité des systèmes d’information.

Dans une logique opérationnelle, ces fonctions restent en retrait. Elles ne “font” pas la sécurité au sens technique du terme, mais elles la traduisent dans la langue de l’entreprise : budgets, lois, contrats, enjeux politiques. Leur rôle, c’est de faire le pont entre ce qui se passe dans le cyberespace et ce qui compte dans la vie réelle des organisations.

La CTI dite 'stratégique' peut aussi être rangée ici, lorsqu’elle produit un renseignement pensé uniquement pour alimenter la gouvernance, sans lien direct avec la détection ou la réponse aux incidents.



## Niveau 0 : Prévention

**Exemples :** administrateurs systèmes et réseaux, architectes sécurité, développeurs, équipes IAM, équipes DevSecOps, pentesters, auditeurs, RSSI techniques

À ce niveau, on retrouve tous ceux qui bâtissent l’infrastructure de défense. Ils installent des pare-feux, segmentent les réseaux, gèrent les identités, appliquent des correctifs… Bref, ils construisent la forteresse.

Le pentest et les audits entrent ici également. Ce sont des activités de contrôle préventif, qui permettent d’évaluer la posture de sécurité à un instant donné. Même si le pentest mobilise des techniques offensives, il reste fondamentalement une activité de prévention, loin des vraies opérations cyber.

Les acteurs de ce niveau restent structurellement passifs : ils agissent avant, sans contact avec les attaques réelles. On est encore en dehors de la chaine opérationnelle.


## Niveau 1 : Détection

**Exemples :** analystes SOC, ingénieurs détection, équipes EDR/XDR, CTI tactique, threat hunting, ingénieurs SIEM, reverser (pour la CTI)

Ici, on entre enfin en contact avec l’attaque. Le rôle de ce niveau est de scruter l’environnement pour repérer les signes de compromission. C’est le premier poste d’observation réel face à l’adversaire.

La détection a besoin de contexte et de direction, fournis par la CTI. Celle-ci permet de calibrer les règles de détection, d’enrichir les alertes, et de formuler des hypothèses de recherche. La CTI est en fait une fonction de soutien à la détection. Dès qu’elle sort de cette logique opérationnelle (pour produire des rapports de haut-niveau sans impact local), elle retombe au niveau -1.

On retrouve également ici le threat hunting, qui est une approche proactive de la détection : ne pas partir d’alertes, mais d’hypothèses de compromission, souvent d'après la CTI. La méthode est différente, mais l'objectif reste le même : la détection.

Le niveau 1 marque le premier point de contact réel avec l’attaque, mais sans action directe sur le système ou sur l’adversaire. C’est la porte d'entrée du monde opérationnel : on observe, on signale, et on transmet à ceux qui vont agir, mais on ne participe pas encore aux opérations elles-mêmes.


## Niveau 2 : Réponse

**Exemples :** équipes CERT/CSIRT, analyste forensic, reverser (pour la réponse)

À ce niveau, on ne se contente plus de regarder : on agit. Les équipes de réponse interviennent directement sur l’attaque en cours. Elles manipulent les systèmes compromis, cherchent la cause, mesurent l’impact, et travaillent à restaurer l’intégrité de l’environnement.


## Niveau 3 : Offensif réel

**Exemples :** exploit developers offensifs, opérateur offensif, APT, hackeur

Tout en haut de la hiérarchie, on trouve ceux qui créent la menace On ne parle pas ici des pentesters, on parle d’opérateurs qui agissent sur des systèmes réels sans autorisation. Ils n'ont pas de règle du jeu, c'est eux le jeu.

Dans toute cette chaine, c’est le seul niveau véritablement proactif, le seul qui n’existe pas en réaction à un autre. Tous les autres métiers de la cybersécurité ne sont là que parce que ce niveau existe. Dans cette hiérarchie, l’offensif réel est la cause première. Il agit. Les autres suivent.


## Conclusion

Cette hiérarchie ne parle pas de salaires, de titres ou de budgets. Elle parle de quelque chose de plus fondamental : dans la cybersécurité, tout tourne autour de l’attaque. C’est elle qui définit le rythme, c’est elle qui oblige à détecter, à répondre, à prévenir et à gouverner.

Plus on est proche de l’attaque, plus on gagne en liberté et en créativité. À l’inverse, plus on s’en éloigne, plus on entre dans des rôles normés et contraints.  

Les grandes attaques marquent l’histoire. On se souvient de Stuxnet, de Conficker, de Mitnick. Mais qui se souvient du RSSI si bon qu'il n’a jamais eu d’incident ? Personne. Dans un écosystème bâti autour du conflit, seuls ceux qui créent le conflit laissent une trace.