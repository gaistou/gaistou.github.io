---
layout: post
title: La hiérarchie opérationnelle des métiers de la cyber
tags: [cyber]
---

Ces dernières années, la cybersécurité s’est fragmentée en une multitude de métiers. Les rôles se sont spécialisés, les postes se sont normalisés, chacun a obtenu un titre, une fiche de poste, et une place bien définie dans la grande machine cyber. Résultat : on a construit un écosystème dense et structuré … mais parfois compliqué à comprendre. Même de l’intérieur.

Je propose ci-dessous un modèle hiérarchique des métiers de la cyber, non pas en termes de statut ou d’importance, mais en terme de proximité avec les opérations. Le point de départ est simple, il n'y a qu'un seul acteur libre dans l'écosystème : c'est l'attaquant. C'est lui qui justifie tout le reste de la chaîne. Sans attaque, il n'y a pas d'industrie de la cyber.

A partir de l'attaquant, on peut établir une chaine hiérarchique opérationnelle, dans laquelle chaque étage est justifié par l'étage directement au dessus :
- Pas d’attaque → pas d'incident.
- Pas d'incident→ pas de détection.

Et en-dessous, on peut encore ajouter deux couches non opérationnelles : la prévention et la gouvernance. Elles ne sont pas directement dans la chaîne de réaction, mais elles existent aussi seulement parce qu'on craint les attaques. Sans menace, pas besoin d’imposer des règles, ni de prévenir quoi que ce soit.


## Niveau -1 : Conformité & Gouvernance

**Exemples :** GRC, DPO, responsables conformité, RSSI purement administratifs, CTI stratégique

Ce niveau regroupe tous les rôles qui gravitent autour de la sécurité sans jamais entrer en contact avec une attaque. Leur production est majoritairement documentaire : politiques, tableaux de risques, grilles d’évaluation, schémas organisationnels. Ils posent le cadre administratif et réglementaire autour de la sécurité des systèmes d'information.

Ces fonctions ont une utilité organisationnelle, mais dans une lecture opérationnelle, elles restent déconnectées de l’action. Ce sont des fonctions de support, qui existent pour justifier ce que d’autres font sur le terrain. Elles ont le rôle de faire le lien entre le cyberespace et "la vraie vie" faite de budgets, de lois, de contrats, et d'enjeux politiques.

La CTI dite 'stratégique' peut aussi être rangée ici, lorsqu’elle produit un renseignement pensé uniquement pour alimenter la gouvernance, sans lien direct avec les opérations techniques ou la réponse aux incidents.



## Niveau 0 : Prévention

**Exemples :** administrateurs systèmes et réseaux, architectes sécurité, développeurs, équipes IAM, équipes DevSecOps, pentesters, auditeurs, RSSI techniques

Ce niveau regroupe tous ceux qui construisent les infrastructures pour limiter les surfaces d’attaque. Ils mettent en place des pare-feux, segmentent les réseaux, gèrent les identités, sécurisent les chaînes CI/CD, appliquent les correctifs. Bref, ils construisent la forteresse.

Le pentest et les audits entrent ici également. Ce sont des activités de contrôle préventif, qui permettent d’évaluer la posture de sécurité à un instant donné. Même si le pentest mobilise des techniques offensives, il reste fondamentalement une activité de prévention, loin des vraies opérations cyber.

Les acteurs de ce niveau restent structurellement passifs : ils agissent avant, sans contact avec les attaques réelles. On est encore en dehors de la chaine opérationnelle.


## Niveau 1 : Détection

**Exemples :** analystes SOC, ingénieurs détection, équipes EDR/XDR, CTI tactique, threat hunting, ingénieurs SIEM, reverser (pour la CTI)

Ce niveau regroupe tous ceux qui scrutent l’environnement pour repérer les signes d’une attaque. Ils filtrent le bruit, trient les signaux faibles, analysent des anomalies, investiguent des comportements suspects. Leur rôle est d’identifier ce qui pourrait être une compromission en cours.

La détection a besoin de contexte et de direction, fournis par la CTI. Celle-ci permet de calibrer les règles de détection, d’enrichir les alertes, et de formuler des hypothèses de recherche. La CTI est en fait une fonction de soutien à la détection. Dès qu’elle sort de cette logique opérationnelle (pour produire des rapports de haut-niveau sans impact local), elle retombe au niveau -1.

On retrouve également ici le threat hunting, qui est une approche proactive de la détection : ne pas partir d’alertes, mais d’hypothèses de compromission, souvent d'après CTI. La méthode est différente, mais l'objectif reste le même : la détection.

Le niveau 1 marque le premier point de contact réel avec l’attaque, mais sans action directe sur le système ou sur l’adversaire. C’est la porte d'entrée du monde opérationnel : on observe, on signale, on transmet à ceux qui vont agir, mais on ne participe pas encore aux opérations elles-mêmes.


## Niveau 2 : Réponse


**Exemples :** équipes CERT/CSIRT, analyste forensic, incident handler, reverser (pour la réponse)

Ce niveau regroupe ceux qui interviennent directement sur l’attaque en cours. Ils manipulent les systèmes compromis, analysent les causes, mesurent l’impact, restaurent l’intégrité. Contrairement aux niveaux précédents, ils ne se contentent pas d’observer : ils agissent.

On est ici au cœur de l’opérationnel : c’est à ce stade seulement, qu’on peut vraiment parler d'opération de cyberdéfense. C’est ce niveau qui donne du sens à tout ce qui précède (détection, prévention, gouvernance) en les rendant utiles face à une attaque réelle.


## Niveau 3 : Offensif réel

**Exemples :** exploit developers offensifs, opérateur offensif, APT, hackeur

Ce niveau regroupe ceux qui créent la menace. On ne parle pas ici des pentesters classiques, encadrés par un cahier des charges, on parle d’opérateurs qui agissent sur des systèmes réels sans autorisation. Ils n'ont pas de règle du jeu, c'est eux le jeu.

Dans toute cette chaine, c’est le seul niveau véritablement proactif, le seul qui n’existe pas en réaction à un autre. Tous les autres métiers de la cybersécurité ne sont là que parce que ce niveau existe. Dans cette hiérarchie, l’offensif réel est la cause première. Il agit. Les autres suivent.


## Conclusion

Cette hiérarchie ne dit rien sur les salaires, ni sur les budgets, ni sur les titres. Elle ne reflète pas la place d’un rôle dans l’organigramme, ni son poids politique dans l’entreprise. Elle dit autre chose : elle parle de liberté d’action, et de rapport direct au réel.

Plus on monte dans cette hiérarchie, plus on gagne en autonomie et en capacité à innover et surprendre. On passe de rôles prescrits à des fonctions où l’action individuelle peut vraiment changer quelque chose. Et je pense qu'avec cette liberté vient naturellement une certaine forme de prestige.

Les grandes attaques marquent les esprits et restent longtemps dans les mémoires. On parle encore aujourd'hui de Mitnick, de Conficker, de Stuxnet. Mais qui se rappellerait du RSSI d'un SI si bien défendu qu'il n'a jamais été attaqué ? Personne. Les couches les plus basses peuvent être efficaces et rigoureuses, mais elles ne font que suivre une dynamique impulsée par ceux qui agissent au-dessus d’elles. Ce n’est pas une question de valeur humaine. C’est juste que dans un écosystème structuré autour du conflit, ceux qui agissent sur le réel sont ceux que l’histoire retient.