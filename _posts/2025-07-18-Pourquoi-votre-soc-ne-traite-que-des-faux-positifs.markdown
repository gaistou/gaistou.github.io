---
layout: post
title: Pourquoi ton SOC ne traite que des faux-positifs
tags: [cyber, detection]
math: true
---


Mon SOC ne traite que des faux positifs. Ton SOC ne traite que des faux positifs. Son SOC ne traite que des faux positifs. Partout où l'on regarde dans le monde de la cyberdéfense, c’est la même rengaine : le SOC croule sous les faux positifs. Et on ne parle pas d’un petit bruit de fond, non non. On parle de centaines, parfois de milliers d’alertes inutiles par jour.

Alors évidemment, on a tous les mêmes suspects habituels :
- "Mon EDR est pourri, je comprends pas qu’on paye Crowdstrike Carbon360 Sentinel machin une fortune pour ça."
- "Les règles de détection sont si nulles qu'on dirait qu’elles ont été pondues par un stagiaire sorti de 42."
- "L’antispam est à la rue, l’antivirus aussi… bla bla bla."

On aime se bercer de l’illusion qu’avec des règles bien pensées, des outils bien configurés et des analystes compétents, la détection serait efficace. Mais si tout ça relevait du fantasme ? Et si le problème était plus profond, plus systémique… voire mathématique ?

J'ai travaillé dans plusieurs SOC, et ça fait quelques années que je rumine le paradoxe de Bayes. Et aujourd’hui, j’en suis arrivé à une conclusion qui va en décevoir plus d’un : la détection, telle qu’on l’imagine dans nos SOC, est mathématiquement vouée à l’échec. Pas parce qu’on est mauvais. Mais parce que le problème est intrinsèquement mal posé.

Alors dans cet article, je vais vous parler de Bayes, de proba, de malveillance, en restant le plus pédagogique possible ne vous en faites pas. En conclusion je vous proposerai une autre manière de penser la détection, une façon plus formalisée, moins naïve, car bien sûr tout n'est pas à jeter. Vous avez 5 minutes ? On y va.

## Le paradoxe de Bayes

Vous avez peut-être déjà entendu parler de ce qu’on appelle le « paradoxe de Bayes ». Le nom est mensonger, ce n’est pas vraiment un paradoxe, c'est juste une erreur d’intuition très courante face aux probabilités conditionnelles. Mais cette erreur est suffisamment fréquente pour avoir déjà foutu en l'air un paquet de décisions médicales, juridiques… et aujourd'hui de cybersécurité.

L'exemple classique vient de l'épidémiologie. On pose la question suivante : "Imaginez que vous avez été diagnostiqué positif pour le SIDA. On vous annonce que le test ne fait que 1% de faux-positif. Quelle est votre chance de réellement avoir le SIDA ?"

Intuitivement, on a envie de répondre : "bah... 99%, non ?".

Raté ! Et pas qu'un peu.

Parce qu'il vous manque une donnée cruciale pour pouvoir répondre à la question : la prévalence de la maladie. C’est-à-dire, à quel point la maladie est fréquente dans la population générale. Et c'est là que ça fait mal. Avec une faible prévalence (ce qui est le cas du VIH aujourd’hui en France), même un test très fiable produira plus de faux positifs que de vrais positifs. En réalité, en France, vos chances d’avoir le VIH après un test positif sont autour de 10%, malgré la "fiabilité" apparente du test.

Alors, vous commencez à voir où je veux en venir ?


## Oui mais attends... que vient faire la prévalence dans cette histoire ?

Pas besoin de sortir les équations pour se convaincre que la prévalence compte. Deux cas extrêmes suffisent :
- Si la prévalence est nulle (personne n'a le SIDA dans la population), alors tous les résultats positifs au test sont forcément faux. Tous les positifs sont des faux-positifs.
- Si la prévalence est maximale (toute la population a le SIDA), alors le test ne peut faire aucun faux-positif, car tous les positifs sont forcément vrais. Tous les positifs sont des vrai-positifs.

Oui ça peut faire bizarre, mais il n'y a pas d'arnaque. C'est l'éternel problème de l'aiguille dans la botte de foin, et plus il y'a de foin, plus il y'a de chances de faire des erreurs de détection. Pour une démonstration mathématique rigoureuse je vous redirige vers cet article très bien écrit : [todo]

Le paradoxe est nommé d'après la formule de Bayes, qui est au final la seule formule qui permette de penser rationnellement à tous les problèmes de tests de détection.

$$
\begin{equation}
P(M \mid A) = \frac{P(A \mid M) \cdot P(M)}{P(A \mid M) \cdot P(M) + P(A \mid \neg M) \cdot P(\neg M)}
\label{eq:bayes}
\end{equation}
$$

Appliquée à un SOC :

- $$P(M \mid A)$$ : la probabilité qu’il y ait vraiment une menace sachant qu’une alerte a été levée.
- $$P(A \mid M)$$ : la probabilité que le système déclenche une alerte s’il y a une menace (*sensibilité*).
- $$P(A \mid \neg M)$$ : la probabilité que le système déclenche une alerte alors qu’il n’y a pas de menace (*taux de faux positifs*).
- $$P(M)$$ : la probabilité a priori qu'il y ait une menace (*prévalence*).
- $$P(\neg M) = 1 - P(M)$$ : la probabilité a priori qu’il n’y ait pas de menace.

Évidemment, c'est pas une formule qu'on peut facilement calculer dans sa tête tous les jours. Mais elle existe, et elle donne une réalité mathématique aux problèmes de détection. Et cette réalité elle est cinglante : ce n’est pas ton EDR, ni ton analyste, ni ta règle YARA qui sont “nuls”... c’est juste que dans un monde où la prévalence de la malveillance dans les données est faible, la proba d’avoir une vraie alerte est mathématiquement faible aussi, même si tout fonctionne bien.

## Le Paradoxe de Bayes m'a tuer

Alors, à quel point la formule de Bayes fait mal à ton SOC ? Prenons un exemple pour s'en faire une idée.

- On cherche à détecter un événement malveillant dans nos logs.
- On ne peut pas le savoir à l'avance dans la vraie vie, mais supposons que 1 événement sur 1 million est malveillant dans nos données. 
- Supposons qu'on a une règle de détection avec 99.99 % de sensibilité, c’est-à-dire qu’elle détecte 99.99 % des vrais positifs, et fait un faux positif 1 fois sur 10 000.

La règle de détection vient de sonner. Quelle est la probabilité que ce soit une vraie alerte ? Posé autrement : quelle est la probabilité qu’il y ait réellement une menace, sachant qu’il y a une alerte ?

On utilise notre formule de Bayes :

$$
\begin{equation}
P(M \mid A) = \frac{P(A \mid M) \cdot P(M)}{P(A \mid M) \cdot P(M) + P(A \mid \neg M) \cdot P(\neg M)}
\label{eq:bayes_alert}
\end{equation}
$$

On remplace :

$$
P(M \mid A) = \frac{0.9999 \cdot 0.000001}{0.9999 \cdot 0.000001 + 0.0001 \cdot 0.999999}
$$

Calcul du numérateur :

$$
0.9999 \cdot 0.000001 = 0.0000009999
$$

Calcul du dénominateur :

$$
0.0000009999 + (0.0001 \cdot 0.999999) = 0.0000009999 + 0.0000999999 = 0.0001009998
$$

Résultat final :

$$
P(M \mid A) = \frac{0.0000009999}{0.0001009998} \approx 0.0099
$$

Résultat : environ 0.99 %. Moins de 1 % de chances que l’alerte soit réelle. Et encore on est parti de suppositions très optimistes (il y a probablement 0 événement malveillant sur votre système en réalité). Et ça c’est juste la réalité statistique, pas un bug de ton SIEM.

Et voilà comment Bayes a tué ton SOC. Pas tes analystes. Pas tes outils. Juste les maths.

## Conclusion

Oui, c’est déprimant. Moi ça me déprime. Profondément.

Aujourd’hui, les SOC sont structurellement condamnés à traiter majoritairement des faux positifs. Pas par incompétence. Pas par faute d’outils. Mais parce que la réalité statistique du monde numérique est contre nous.

Je ne dis pas ça pour le plaisir de détruire. J’aimerais que mon travail ait du sens. J’aimerais que la détection fonctionne. Mais il faut être lucide : notre manière actuelle de concevoir la détection est inefficace, naïve, et mathématiquement vouée à l’échec.

Dans un prochain article, je vous proposerai une autre manière de penser le rôle du SOC. Un monde où l’objectif n’est plus de “détecter la malveillance” (tâche perdue d’avance, on vient de le voir) mais de cartographier, qualifier, et monitorer la normalité. Un monde où l’analyste ne chasse plus les loups, mais mesure la stabilité du troupeau.

C’est peut-être moins sexy. Mais au moins, ça a une chance de marcher.