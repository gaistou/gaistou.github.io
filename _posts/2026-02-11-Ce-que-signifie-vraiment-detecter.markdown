---
layout: post
title: La détection d'intrusion sous l'angle du théorème de Bayes
tags: [cyber, detection]
math: true
---


Dans [l’article précédent](https://gaistou.github.io/posts/Comment-evaluer-un-modele-de-detection/), nous avons exploré les métriques classiques d’évaluation des systèmes de détection : la sensibilité, la spécificité et la précision. Nous sommes partis de la matrice de confusion et avons reconstruit, pas à pas, tout le raisonnement permettant d’évaluer un IDS. La conclusion était peu satisfaisante, mais mathématiquement solide : dès que la prévalence des attaques est faible, la précision baisse drastiquement. Chercher une aiguille dans une botte de foin, c’est peine perdue : on finit mécaniquement avec des tests bons sur le papier, mais inutilisables en pratique.

Dans cet article, nous allons revoir le problème sous un autre angle. Nous verrons que nos métriques d’évaluation (sensibilité, spécificité, précision) peuvent toutes être interprétées comme des probabilités conditionnelles, que l’on peut manipuler avec la formule de Bayes et des _likelihood ratios_. Ce changement de point de vue ne changera rien au problème de la prévalence, mais il le rendra beaucoup plus intuitif. Avec cette compréhension améliorée, on pourra ensuite concevoir des SOC qui comprennent réellement ce que signifie « détecter ».

Cet article est donc une transition théorique : nous avons déjà constaté les limites des tests de détection ; nous allons maintenant mieux les comprendre pour mieux les affronter dans les prochains articles.


# 1. Les ratios sont des probabilités conditionnelles


Prenons l’exemple de la sensibilité. Jusqu’ici, nous l’avons définie comme le nombre de tests positifs rapporté à l’ensemble des événements malveillants testés, c’est-à-dire la proportion d’événements malveillants effectivement détectés, ou plus formellement : $\frac{VP}{VP + FN}$. Mais on peut aussi dire que c’est la probabilité qu’un événement soit testé positif sachant qu’il est malveillant, que l’on note $P(T^+ \mid M)$. Les probabilités et les ratios sont toujours deux facettes d’une même pièce.

On peut donc réécrire toutes nos métriques comme des probabilités conditionnelles, avec $M$ pour « malveillant » et $T^+$ ou $T^-$ pour désigner le résultat du test.

On en déduit :
- La sensibilité $\Leftrightarrow$ $$P(T^+ \mid M)$$ $\Leftrightarrow$ la probabilité qu’un événement soit testé positif sachant qu’il est malveillant.
- La spécificité $\Leftrightarrow$ $$P(T^- \mid \neg M)$$ $\Leftrightarrow$ la probabilité qu’un événement soit testé négatif sachant qu’il n’est pas malveillant.
- La précision $\Leftrightarrow$ $$P(M \mid T^+)$$ $\Leftrightarrow$ la probabilité qu’un événement soit malveillant sachant qu’il a été testé positif.



# 2. La formule de Bayes

L’outil de base pour calculer des probabilités conditionnelles, on l’apprend au lycée : c’est le théorème de Bayes.

$$P(A \mid B) = \frac{P(A) \times P(B \mid A)}{P(B)}$$

Par exemple, si l’on applique cette formule pour calculer la précision, c’est-à-dire $$P(M \mid T^+)$$, on obtient :

$$P(M \mid T^+) = \frac{P(T^+ \mid M) \times P(M)}{P(T^+)}$$

Puis, pour montrer ce qu’il se passe lorsque l’on pousse la formule jusqu’au bout, on peut décomposer le dénominateur avec la loi des probabilités totales :

$$P(M \mid T^+) = \frac{P(T^+ \mid M) \times P(M)}{P(T^+ \mid M) \times P(M) + P(T^+ \mid \text{non } M) \times P(\text{non } M)}$$

On peut maintenant observer l’équation et essayer d’interpréter chacun des termes. On voit :
- $$P(M \mid T^+)$$ : la probabilité qu’un événement soit malveillant sachant qu’il a été testé positif, c’est-à-dire la précision.
- $$P(T^+ \mid M)$$ : la probabilité qu’un événement soit détecté sachant qu’il est malveillant, c’est-à-dire la sensibilité.
- $$P(M)$$ : la probabilité qu’un événement pris au hasard soit malveillant, c’est-à-dire la prévalence.
- $$P(T^+ \mid \text{non } M)$$ : la probabilité qu’un événement déclenche une alerte sachant qu’il n’est pas malveillant, c’est-à-dire $$(1 - \text{spécificité})$$.

Et là, tout apparaît clairement. Quand on s’exprime en probabilités, on voit immédiatement que la précision dépend de la sensibilité, de la prévalence et de la spécificité. On retrouve en réalité la même formule que dans l’annexe de mon premier article, mais par un chemin beaucoup plus simple. On peut l’écrire proprement ainsi :

$$\text{précision} = \frac{\text{sensibilité} \times \text{prévalence}}{\text{sensibilité} \times \text{prévalence} + (1 - \text{spécificité}) \times (1 - \text{prévalence})}$$

Pour rappel, on peut reprendre la matrice de confusion de l’article précédent :


|                               | Prédiction : malveillant | Prédiction : non malveillant |
|-------------------------------|--------------------------|------------------------------|
| **Réalité : malveillant**     | VP = 999                 | FN = 1                       |
| **Réalité : non malveillant** | FP = 99 999              | VN = 99 899 001              |


Et appliquer la formule pour retrouver à nouveau notre précision.

$$
\text{precision} = \frac{0{,}999 \times 0{,}00001}{0{,}999 \times 0{,}00001 + (1 - 0{,}999) \times (1 - 0{,}00001)}
$$

$$
\approx 1 \%
$$

On obtient exactement le même résultat que dans mon premier article, toujours aussi mauvais. Évidemment, on a simplement changé de formule pour calculer la même chose ; en l’état, cela ne résout aucun problème.


## 3. And so what ... ?

 Vous êtes peut-être en train de vous demander : "Mais pourquoi s’embêter à mettre des probabilités partout  si ça ne résout rien ? Pourquoi rajouter des nouvelles formules pour avoir les même résultats ?". On y vient !

L'intérêt c'est surtout de voir les choses autrement. D'avoir un nouveau cadre mental pour penser la détection :

- Premièrement, le raisonnement devient beaucoup plus intuitif. _La probabilité qu’un événement soit malveillant sachant qu’il a levé une alerte_ est un concept immédiatement compréhensible, bien plus parlant que le mot _précision_ qui lui est presque un faux-ami. 

- Deuxièmement, l'écriture sous forme de probabilités met en évidence le rôle des métriques. La sensibilité et la spécificité sont conditionnées par la réalité (*sachant* M ou *sachant* non M), alors que la précision est conditionnée par le résultat du test (*sachant* T+). Autrement dit, **la sensibilité et la spécificité nous informent sur le test selon la réalité, alors que la précision nous informe sur la réalité selon le test.** 

- Troisièmement, on va pouvoir calculer des évolutions de probabilités et comprendre ce que ça veut réellement dire de *détecter* quelque chose.


## 4. Introduction aux odds

Avant de passer à la partie suivante, il faut que je vous introduise la différence entre les _odds_ et les probabilités.

Une probabilité mesure la chance qu’un événement se produise, entre 0 et 1. Dans notre cas, par exemple, $$P(M)$$ vaut $$\frac{1}{100\,000}$$, car j’ai 99 999 événements non malveillants pour 1 événement malveillant. Cette idée de _99 999 contre 1_ s’appelle une _odds_. On peut l’écrire : $$\text{odds}(M) = \frac{1}{99\,999}$$ et la prononcer _1 contre 99 999_. C’est exactement le même concept que les cotes que l’on retrouve dans les paris sportifs.

On a une formule à connaître pour passer d’une probabilité à une _odds_ :

$$
\text{odds}(A) = \frac{P(A)}{1 - P(A)}
$$

Si on l’applique à notre problème, on obtient :

$$
\text{odds}(M) = \frac{P(M)}{1 - P(M)}
= \frac{\frac{1}{100\,000}}{1 - \left(\frac{1}{100\,000}\right)}
= \frac{1}{99\,999}.
$$

On retrouve bien le bon résultat.

Bien sûr, cela ne change rien : on parle toujours du même concept, 1 événement parmi 100 000. Mais maintenant, on a deux façons de l’écrire :

$$
P(M) = \frac{1}{100\,000}
\quad \text{ou} \quad
\text{odds}(M) = \frac{1}{99\,999}.
$$


Je ne vais pas entrer dans les détails des raisons pour lesquelles on voudrait parler en probabilités ou en _odds_ en général, mais sachez que ce sont deux représentations mathématiques différentes. Certains raisonnements sont plus intuitifs en probabilités, d’autres le sont davantage en _odds_. Et cela tombe bien : on connaît un très bon exemple de formule intéressante à convertir en _odds_ : le théorème de Bayes.



## 5. Du théorème de Bayes au Likelihood Ratio


On a vu dans la partie 2 que le théorème de Bayes nous permet d’arriver facilement à :

$$\text{précision} = P(M \mid T^+) = \frac{\text{sensibilité} \times \text{prévalence}}{\text{sensibilité} \times \text{prévalence} + (1 - \text{spécificité}) \times (1 - \text{prévalence})}$$

On a dit que ce n’était pas mal, car cette formule mettait bien en avant que la précision dépend de la prévalence, de la sensibilité et de la spécificité. Mais on peut en fait faire encore **beaucoup** mieux en raisonnant en _odds_. Je vous épargne la démonstration (disponible en annexe 1) et je vous donne tout de suite le résultat :

$$\text{odds}(M \mid T^+) = \left(\frac{\text{sensibilité}}{1 - \text{spécificité}}\right) \times \text{odds}(M)$$

Autrement dit, les _odds_ qu’un événement soit malveillant sachant qu’il a déclenché une alerte, ce sont les _odds_ de la prévalence multipliées par $$\left(\frac{\text{sensibilité}}{1 - \text{spécificité}}\right)$$. Cette dernière valeur est en fait ce que les mathématiciens appellent le _likelihood ratio_, noté $$LR^+$$.

On arrive donc à notre formulation finale du théorème de Bayes en version _odds_ :

$$\text{odds}(M \mid T^+) = \text{odds}(M) \times LR^+$$

En particulier, dans notre cas :

$$
LR^+ = \frac{\text{sensibilité}}{1 - \text{spécificité}}
\approx \frac{0{,}999}{0{,}001}
\approx 998.
$$


Si l’on reformule tout cela simplement en français : une alerte sur un événement multiplie ses _odds_ d’être malveillant par 998.


## 6. Ce que fait réellement un test de détection


On va maintenant revenir à notre point de départ et réécrire le problème sous forme d’_odds_, armés de notre nouvelle formule.

- Au commencement, on a 100 000 000 d’événements avec une prévalence de la malveillance de $$\frac{1}{100\,000}$$. Si l’on y pioche un événement au hasard dans nos données, les _odds_ que l’événement soit malveillant sont donc de 1 contre 99 999, c’est-à-dire :
  $$\text{odds}(M) = \frac{1}{99\,999}.$$

- Puis on passe chaque événement dans un test de détection avec 99,9 % de sensibilité et 99,9 % de spécificité, et on obtient $$\sim 100\,000$$ alertes. Si l’on y pioche une alerte au hasard, les _odds_ que l’alerte corresponde à un événement malveillant sont :  
  $$\text{odds}(M \mid T^+) = \text{odds}(M) \times LR^+ = \frac{1}{99\,999} \times 998 \approx \frac{1}{100}.$$

Nos alertes remontent des événements qui ont 998 fois plus de chances d’être malveillants qu’un événement pris au hasard dans l’ensemble des données. **C’est cela, la vérité mathématique de la détection : un test de détection remonte des événements qui ont “plus de chances d’être malveillants”.** Et dans notre cas, c’est 998 fois plus de chances. **Et donc, quand la prévalence est presque nulle, 998 fois presque rien, cela donne encore presque rien.**

Et là, ça y est : on a enfin une intuition correcte du problème de la détection. La précision qui s’effondre face à une faible prévalence n’est plus une bizarrerie mathématique, c’est une évidence.



## 7. Conclusion

Nous pouvons maintenant reformuler proprement le problème de la détection.

Il ne faut pas voir un système de détection comme un oracle. Fondamentalement, ce n’est qu’un modificateur de probabilités. Il transforme les _odds_ _a priori_ que l’événement soit malveillant en _odds_ _a posteriori_. Ce multiplicateur dépend de la performance du test, c’est-à-dire de sa sensibilité et de sa spécificité, et porte un nom précis : le _likelihood ratio_.

Pour obtenir une précision satisfaisante, il faut donc un LR+ du même ordre de grandeur que l’inverse de la prévalence. Dans le domaine de la détection d’intrusion, où la prévalence est extrêmement faible, on arrive malheureusement toujours à la même conclusion : aucun système de détection généraliste n’atteindra un LR+ suffisamment grand ; chercher une aiguille dans une botte de foin est toujours perdu d’avance.

Mais nous avons maintenant une intuition beaucoup plus saine de ce qu’est réellement la détection, ainsi que de nouveaux outils mathématiques pour développer des solutions.

Je vous avoue qu’au moment où j’écris ces lignes, je ne sais pas si l’on finira par trouver une méthode robuste pour faire de la détection à faible prévalence. Mais j’ai deux pistes en tête qui semblent prometteuses et que nous explorerons dans les prochains articles :
- Peut-on cumuler les tests de détection ? Pour quel gain ?
- Serait-il possible de détecter la malveillance sans réellement détecter d’événement malveillant ?


## Annexe 1

On l'a vu, sous forme de probabilité, la précision s'écrit $$P(M \mid T^+)$$.

On sait que :

$$
\text{odds}(M \mid T^+) = \frac{P(M \mid T^+)}{1 - P(M \mid T^+)}
$$
$$
= \frac{P(M \mid T^+)}{P(\text{non } M \mid T^+)}
$$
             
D'après le théorème de Bayes :

$$
P(M \mid T^+) = \frac{P(T^+ \mid M) * P(M)}{P(T^+)} \;;\ \text{et}
$$
$$
P(\text{non } M \mid T^+) = \frac{P(T^+ \mid \text{non } M) * P(\text{non } M)}{P(T^+)}
$$

Donc :

$$
\text{odds}(M \mid T^+) =
\frac{\left(\frac{P(T^+ \mid M) * P(M)}{P(T^+)}\right)}{\left(\frac{P(T^+ \mid \text{non } M) * P(\text{non } M)}{P(T^+)}\right)}
$$

On simplifie en haut et en bas par $$P(T^+)$$ :

$$
\text{odds}(M \mid T^+) =
\frac{P(T^+ \mid M) * P(M)}{P(T^+ \mid \text{non } M) * P(\text{non } M)}
$$

On peut couper la formule en 2 :

$$
\text{odds}(M \mid T^+) =
\left(\frac{P(T^+ \mid M)}{P(T^+ \mid \text{non } M)}\right)
*
\left(\frac{P(M)}{P(\text{non } M)}\right)
$$
$$
=
\left(\frac{P(T^+ \mid M)}{P(T^+ \mid \text{non } M)}\right)
*
\left(\frac{P(M)}{1 - P(M)}\right)
$$

On reconnait tout à droite une expressions de odds :

$$
\text{odds}(M \mid T^+) =
\left(\frac{P(T^+ \mid M)}{P(T^+ \mid \text{non } M)}\right)
*
\text{odds}(M)
$$

On sait que :

$$P(T^+ \mid M)$$ c'est la sensibilité ; et  
$$P(T^+ \mid \text{non } M)$$ c'est $$1 - \text{specificité}$$

$$\text{précision (formulée comme une odds)} = \text{odds}(M \mid T^+) = \left(\frac{\text{sensibilité}}{1 - \text{spécificité}}\right) \times \text{odds}(M)$$