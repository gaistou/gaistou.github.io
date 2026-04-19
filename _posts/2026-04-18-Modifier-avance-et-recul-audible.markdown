---
layout: post
title: Modifier le bouton d'avance rapide sur Audible
tags: [divers, web]
math: true
---

TL;DR : voici un script Tampermonkey pour modifier le bouton de recul rapide du lecteur Web de Audible afin qu'il ne recule que de 5 secondes au lieu de 30 :


```javascript
// ==UserScript==
// @name         Audible skip back 5s
// @match        https://www.audible.fr/webplayer*
// @run-at       document-idle
// ==/UserScript==

(function () {
  function getMedia() {
    return document.querySelector('audio, video');
  }

  function isSkipBackButton(target) {
    return target && target.closest && target.closest('[data-testid="skip-back"]');
  }

  document.addEventListener('click', function (e) {
    const button = isSkipBackButton(e.target);
    if (!button) return;

    const media = getMedia();
    if (!media) {
      console.log('Audible skip 5s: média introuvable');
      return;
    }

    e.preventDefault();
    e.stopPropagation();
    e.stopImmediatePropagation();

    media.currentTime = Math.max(0, media.currentTime - 5);
    console.log('Audible skip 5s:', media.currentTime);
  }, true);
})();
```

J’ai découvert il y a quelques années que j’étais capable de jouer à Diablo III tout en écoutant un livre audio. Depuis, c’est devenu ma principale motivation pour lire. Je trouve un jeu pas trop demandant intellectuellement (typiquement un hack’n’slash), et je me lance dans 30 heures de lecture audio d’un vieux classique que je n’aurais probablement jamais réussi à lire autrement. J’allie le meilleur des deux mondes : le jeu me stimule suffisamment pour rester concentré et satisfait pendant de longues périodes, tout en développant ma culture littéraire.

Le problème, bien sûr, c’est que je ne suis pas toujours attentif à 100 % à la lecture, et que je dois parfois faire de petits retours en arrière pour rattraper une phrase. Malheureusement, le bouton retour arrière du lecteur web (ou “cloud”) d’Audible fait un bond de 30 secondes en arrière, impossible à personnaliser. Je me retrouve donc à devoir me retaper 30 secondes déjà écoutées alors que seules les quelques dernières secondes m’intéressent.

Aujourd’hui, j’en ai eu marre, et j’ai décidé d’essayer de comprendre comment fonctionne le lecteur Audible pour forcer moi-même cette modification.

Je précise tout de suite : je ne suis expert ni en front-end, ni en reverse engineering. C’est même plus ou moins la première fois que je me lance sérieusement dans ce genre d’exploration.

Cet article a deux objectifs :

- montrer une façon de modifier le bouton d’Audible pour qu’il recule de 5 secondes au lieu de 30 ;
- montrer un exemple de reverse engineering front-end sur une application moderne, vu par quelqu’un qui découvre le sujet.

Je pourrais vous emmener directement à la solution qui marche (celle du TL;DR). Ici, l’idée de l'article est plutôt d’explorer le problème étape par étape, de voir jusqu’où on peut aller avec les outils du navigateur et un peu de curiosité, et de découvrir ensemble les contraintes de ce genre d’exercice.


## Etape 1 : Comprendre le bouton de recul rapide

Avec un tout petit peu d’expérience en développement web, on se doute que c’est une fonction en Javascript qui applique le recul rapide lorsqu’on clique sur le bouton. Mon premier réflexe est donc d’inspecter le bouton (Ctrl + Shift + C) et de repérer la fonction Javascript associée.

![](/assets/images/audible1.png){: width="1000" height="1000"}

On retrouve bien notre bouton “skip-back”, qui est associé à un *event listener* en Javascript. On peut cliquer sur le petit bouton “event” pour obtenir le détail de ce listener.

![](/assets/images/audible2.png){: width="1000" height="1000"}

L’event listener détecte un clic de souris et appelle la fonction `Jc()` qui… ne fait rien ?

On comprend que ce listener ne sert en fait à rien. C’est juste un artefact de développement : la fonction `Jc()` sert peut-être à quelque chose dans le fonctionnement global d’Audible, mais ce n’est certainement pas elle qui provoque le recul rapide. Il doit y avoir un autre listener, ailleurs, qui détecte le clic et qui déclenche réellement l’action.


## Etape 2 : A la recherche du bon *listener*

Pour avoir plus d’informations sur le bouton, on peut afficher sa représentation dans le DOM. Pour ce faire, on le sélectionne dans l’onglet “Inspecteur”, puis dans l’onglet “Console”, on tape : `$0`.

![](/assets/images/audible3.png){: width="1000" height="1000"}

On retrouve bien la propriété *onclick* associée à la fonction `Jc()` qui ne sert à rien. Mais plus intéressant : on voit les propriétés `__reactProps$z9p4912tsd` et `__reactFiber$z9p4912tsd`, qui indiquent que l’application est faite avec le framework React.

En lisant la documentation de React (enfin, en laissant ChatGPT la lire pour nous), on apprend que React utilise un listener global qui écoute les événements sur la page, puis appelle la fonction déclarée dans les *reactProps* de l’élément concerné.


## Etape 3 : Retrouver la fonction appelée par React

On déplie maintenant la propriété reactProps de notre bouton. On y retrouve effectivement une fonction `h()`. On peut afficher son contenu avec : `$0.__reactProps$z9p4912tsd.onClick.toString()`.

![](/assets/images/audible4.png){: width="1000" height="1000"}

On voit alors le code de la fonction `h()` réellement appelée lorsque l’on clique sur le bouton : `()=>{n(xo(-30))}`.

Pas besoin d’aller chercher ce que sont les fonctions `n()` et `xo()` : on comprend que le -30, codé en dur ici, correspond à la durée du retour en arrière.


## Etape 4 : Retrouver le fichier Javascript qui contient le code

On veut maintenant modifier la valeur -30 pour quelque chose qui nous convient mieux, comme -5. Pour cela, on peut commencer par retrouver le fichier qui contient ce code.

On va dans l’onglet “Débogueur”, puis dans “Rechercher” à gauche, et on entre le code suivant : `()=>{n(xo(-30))}`.

![](/assets/images/audible5.png){: width="1000" height="1000"}

Le bout de code est dans le fichier `images/S/cCRQ4krLAjl4kpL.js`


## Etape 5 : Modifier la fonction h()

Et là… en fait on est bloqué.

Sur le papier, la solution semble évidente : remplacer -30 par -5 dans le fichier Javascript. Autrement dit, faire un simple *sed* sur le fichier quand il est récupéré par le navigateur. Mais en pratique, ce n’est pas si simple.

Le problème vient du fait que le navigateur télécharge et exécute les fichiers Javascript immédiatement. Firefox ne permet pas de modifier le fichier au moment du téléchargement. Puis dès que le fichier Javascript est exécuté, la fonction `h()` est associée au bouton dans le DOM et c'est déjà trop tard pour modifier le fichier Javascript source.

La piste suivante consiste donc à essayer de modifier directement la fonction `h()` depuis le DOM. Mais là encore, ce n’est pas aussi simple qu’on pourrait le croire.

Ce que l’on voit de la fonction `h()` dans le DOM n’est pas du code source modifiable, mais une fonction déjà construite, créée au moment de l’exécution du fichier Javascript. Cette fonction a été initialisée avec la valeur -30 et des dépendances `n()` et `xo()` qui ne sont plus modifiables à ce stade. Autrement dit, pour remplacer -30 par -5, il faudrait rejouer tout le processus de création de cette fonction avec son environnement d’origine, et ça a l'air beaucoup trop compliqué.

Alors piste suivante : si on ne peut pas modifier le code après son arrivée dans le navigateur, on peut essayer de le modifier avant avec un proxy devant notre navigateur :

- lancer un proxy local (par exemple en Python)
- configurer Firefox pour passer par ce proxy
- gérer les certificats pour intercepter le HTTPS
- modifier le fichier Javascript à la volée avant qu’il n’arrive dans le navigateur

Sur le papier, ça sonne bien. Mais en pratique, c’est assez lourd à mettre en place, et ça implique de faire tourner un proxy en permanence sur sa machine, tout ça pour un bouton sur Audible… la flemme. C'est pas ce qu'on voulait à la base.

Conclusion après 2h de bidouillage : impasse.


## Etape 6 : Fuck la fonction h(), ou comment contourner le problème

On l’a vu, modifier une fonction déjà créée n’est pas simple. On va donc changer complètement d’approche : au lieu d’essayer de bricoler la fonction `h()` existante, on va plutôt complétement la remplacer.

Quand on clique sur le bouton, un événement *click* est déclenché. React écoute cet événement et appelle sa fonction `h()`.Mais on peut intercepter cet événement avant React, en ajoutant notre propre listener, avec notre propre fonction.

Voici le code pour ajouter notre listener custom :

```javascript
document.addEventListener('click', (e) => {
    const btn = e.target.closest('[data-testid="skip-back"]');
    if (!btn) return;

    const media = document.querySelector('audio, video');
    if (!media) return;

    e.preventDefault();
    e.stopPropagation();
    e.stopImmediatePropagation();

    media.currentTime = Math.max(0, media.currentTime - 5);

    console.log('[patch] rewind 5s');
}, true)
```

Le code surveille chaque clic sur la page (pour s'appliquer avant la fonction `h()`) :
- on vérifie que le clic est sur le bouton skip-back ; 
- on récupère l'objet qui contrôle la lecture audio ;
- il bloque la propagation de l'événement pour pas que React lance sa fonction `h()` ;
- puis on applique notre propre code : retourner 5 secondes en arrière dans le lecteur audio.

On peut copier-coller le bout de code dans la console, et ça marche ! Le bouton revient bien 5 secondes en arrière au lieu de 30.

Cependant, en passant par la console, cette modification ne dure que jusqu'au rechargement de la page. Il faut donc maintenant automatiser l’injection du script à chaque ouverture du lecteur Audible. Pour ça, l’outil le plus populaire est Tampermonkey.


## Etape 7 : Automatisation avec Tampermonkey

Tampermonkey est une extension de navigateur, disponible sur le store Firefox, qui permet d’injecter du Javascript dans une page web au moment de son chargement.

Voici notre script Tampermonkey final :

```javascript
// ==UserScript==
// @name         Audible skip back 5s
// @match        https://www.audible.fr/webplayer*
// @run-at       document-idle
// ==/UserScript==

(function () {
  function getMedia() {
    return document.querySelector('audio, video');
  }

  function isSkipBackButton(target) {
    return target && target.closest && target.closest('[data-testid="skip-back"]');
  }

  document.addEventListener('click', function (e) {
    const button = isSkipBackButton(e.target);
    if (!button) return;

    const media = getMedia();
    if (!media) {
      console.log('Audible skip 5s: média introuvable');
      return;
    }

    e.preventDefault();
    e.stopPropagation();
    e.stopImmediatePropagation();

    media.currentTime = Math.max(0, media.currentTime - 5);
    console.log('Audible skip 5s:', media.currentTime);
  }, true);
})();
```

Et voila, mission accomplie !

![](assets/images/audible6.png)
