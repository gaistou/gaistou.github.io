---
layout: post
title: J'ai donné les clés de mon vault Obsidian à Claude Code
tags: [recherche, IA]
---

Mon état de l'art suit son cours, toujours en suivant [la méthode que j'ai décrite ici](https://gaistou.github.io/posts/Etat-de-art-avec-jabref-et-obsidian-part-1/). J'ai maintenant 453 notes atomiques, 1901 liens sémantiques et 109 publications de référence. Un beau bébé !

Ces deux derniers mois, un nouvel acteur majeur est entré dans ma méthode : l'IA agentique. Claude Code a considérablement amélioré la fluidité et la qualité de mon travail, et ça mérite un petit retour d'expérience.


## Claude Code pour les nuls

Claude Code est un logiciel en ligne de commande qui permet de parler à Claude directement dans son terminal. Mais contrairement à Claude.ai sur le web, ce n'est plus un simple agent conversationnel : il a la main sur votre ordinateur, agit sur vos fichiers, lance des programmes. Tout ce que votre ordinateur est capable de faire, Claude Code peut le faire.

Le cas d'usage typique, c'est le développement logiciel dans la lignée de ce que faisait déjà Copilot. On lance Claude Code sur un projet, on lui décrit le résultat qu'on veut obtenir, et Claude Code écrit le code tout seul comme un grand. Mais il va bien plus loin : il peut installer et configurer un serveur web pour faire tourner le code, prendre des captures d'écran de l'interface, évaluer ce qu'il voit, revenir modifier le code, committer les changements et pousser en prod. Dans ce cas d'usage, le développeur ne fait plus qu'écrire des prompts. C'est impressionnant.

Le mot « Code » dans son nom n'est pas anodin, il a été conçu pour ça. Mais rien ne le retient au code. Dans mon cas, je l'utilise dans mon vault Obsidian. Il peut ainsi librement lire mes 453 notes, suivre mes 1901 liens, étudier mes 109 publications, et je peux ensuite l'interroger sur tout ça.

J'utilisais déjà ChatGPT avant de passer à Claude Code, avec qui j'avais eu des conversations intéressantes sur mon sujet de thèse. Mais c'était toujours fastidieux de lui redonner le contexte, sur de plus en plus d'éléments, au fur et à mesure que ma thèse avançait. Claude Code, lui, navigue librement dans mon vault Obsidian. Si je maintiens bien mes notes à mon niveau actuel de compréhension et de réflexion, c'est tout comme s'il était directement dans ma tête.


## Du cas d'usage vers le Claude Skill

Au-delà de la discussion libre, Claude m'assiste sur plusieurs tâches récurrentes. Claude Code permet de les définir dans ce qu'on appelle des "skills", qui sont des prompts fixes que j'écris dans un fichier *skill.md* et que je peux ensuite appeler en un mot.

### Evaluation à priori d'un article scientifique

Avant Claude Code, je perdais énormément de temps à lire des articles qui apportaient peu à mon état de l'art. C'est mon quotidien depuis 10 mois : voir un nouvel article sur mon sujet de thèse, et me poser la question *devrais-je lire cet article ?*. Ma méthode classique était quelque chose du genre :
- est-ce que l'auteur est reconnu dans le domaine ?
- est-ce que le journal/conférence est reconnu dans le domaine ?
- lire l'abstract, repérer des mots clés
- lire l'introduction et la conclusion, les auteurs essaient-ils de répondre à une question qui m'intéresse ?
- lire les résultats, est-ce qu'ils arrivent à une réponse intéressante ?
- et si j'arrive jusque-là, alors oui je dois m'arrêter et lire sérieusement l'article.

Mais tout ce process est long, et fastidieux. Un article scientifique, c'est toujours difficile à lire. Il suffit que le sujet soit un peu différent du mien et je peux me retrouver embrumé dans des dizaines de termes techniques que je n'avais jamais vus avant. Le temps de comprendre ces termes et de faire le lien avec mon sujet, la journée est finie. Tout ça pour conclure que finalement l'article était pas si intéressant.

Pour cette tâche, j'ai maintenant un skill Claude *(voir [annexe](#annexe--prompts-des-skills))*. Je lui donne un PDF, et en 30 secondes je sais si je peux passer à autre chose.

![image](/assets/images/claude-eval-article.png){: width="1000" height="1000"}
*Exemple de retour de Claude sur un article récent*


### Confrontation de mes notes de lecture

Quand je lis une publication scientifique sérieusement, je rédige en parallèle une note de lecture. Dans cette note, je relie les concepts abordés par le papier avec mes notes de concepts atomiques. Ces liens me permettent par la suite de retrouver facilement toutes les publications qui abordent un concept donné. Puis j'extrais dans ma note tout ce que je souhaite retenir du papier : les idées intéressantes, les résultats marquants, les nouveaux concepts, les limites, etc.

Sur cette tâche, Claude Code est mon correcteur. Une fois que j'ai fini de rédiger ma note de lecture, je lui demande un avis critique et d'évaluer si j'ai réellement compris ce que voulait dire le papier. Je fais systématiquement appel à ce skill pour chaque nouvelle note, et je suis convaincu que ça a considérablement augmenté leur qualité.

Le skill *(voir [annexe](#annexe--prompts-des-skills))* relit le PDF source et ma note de lecture en parallèle, puis me confronte à ce que j'ai raté ou mal formulé.

![image](/assets/images/claude-note-lecture.png){: width="1000" height="1000"}
*Exemple de retour sur une note de lecture que je viens d'écrire*

Ça me rajoute du boulot de relecture et d'approfondissement, mais toutes ses remarques sont hyper pertinentes !


## Les limites

### La confidentialité

Quand Claude Code lit mon vault Obsidian, mes notes transitent par les serveurs d'Anthropic, une entreprise américaine soumise au CLOUD Act. Ce n'est pas anodin quand on fait de la recherche sur la cybersécurité des systèmes navals. Je ne travaille pas sur des informations classifiées, et l'ensemble des travaux de ma thèse sont *Non Protégé*, mais on pourrait argumenter que ma réflexion est représentative du niveau capacitaire de la France sur Zero Trust, ce qui pourrait être une information sensible.

Je n'en suis encore qu'à l'état de l'art, qui est probablement la partie la moins sensible de la thèse, puisque ce n'est qu'une synthèse d'éléments publics. Mais je devrais certainement réviser ma consommation de Claude Code quand je commencerai à travailler sur ma propre contribution scientifique.

Je fais constamment attention à ce que j'écris dans mes notes, que je dois désormais considérer comme des notes publiques. Concrètement, ça m'a forcé à créer un dossier "Private" dans mon vault Obsidian qui contient mes notes *personnelles* : comptes rendus de réunion, notes concernant les travaux de mes collègues non publiés, réflexions plus personnelles. J'ai ensuite configuré Claude Code pour lui interdire l'accès à ce dossier *Private* (dans .claude/settings.json).

Et bien sûr, je garde en tête que j'ai ainsi accepté d'installer un agent d'IA avec les pleins pouvoirs sur mon PC. Claude Code pourrait à tout moment aller consulter l'ensemble des fichiers de mon ordinateur et les transmettre à Anthropic. Utiliser Claude Code implique une forte confiance en Anthropic, une entreprise américaine.

It sucks, but oh well. J'accepte le risque.


### L'attribution du travail

À la fin de la thèse, on pourrait argumenter que ce n'est pas moi qui suis devenu docteur, mais Claude. Et que je me suis simplement attribué le travail de Claude.

Mais je n'ai pas l'impression de m'attribuer le travail de Claude. Une thèse, ça ne se défend pas en prouvant qu'on a rédigé chaque phrase soi-même sans l'aide de personne. Ça se défend en montrant qu'on maîtrise les idées. Si Claude me suggère un lien entre deux concepts et que je l'intègre à mes notes parce que je l'ai compris, validé, et que je suis capable de l'expliquer, ce n'est pas très différent de si c'était une suggestion de mon directeur de thèse.

Ce qui change avec Claude, c'est la fluidité. Le risque n'est pas qu'il écrive à ma place, c'est que sa confiance et son éloquence m'incitent à valider trop vite ce que je ne maîtrise pas encore vraiment. C'est un risque de paresse intellectuelle, pas un problème d'attribution. Et c'est un risque auquel je suis très sensible. Par exemple, je ne demande jamais à Claude d'écrire mes notes de lecture, c'est seulement mon correcteur.

Je n'oublie pas que l'objectif de la thèse n'est pas seulement de produire un manuscrit, mais de faire de moi un docteur. Et j'utilise Claude davantage comme un formateur que comme un rédacteur.


### La responsabilité

Claude hallucine. Sur des sujets généraux il est relativement fiable, mais sur un sujet aussi pointu que le Zero Trust appliqué aux systèmes OT navals, il peut vite affirmer avec assurance quelque chose de faux. Et ce n'est pas dit que je m'en rende compte immédiatement.

Claude n'est pas ma source d'information. Tout ce qui finit dans mes notes ou dans ma rédaction doit être ancré dans un primaire que j'ai lu. Claude peut m'aider à identifier ce qu'il faut lire, à formuler une idée, à détecter ce que j'ai raté dans une note de lecture, mais il ne remplace pas la source. Même si Claude est éloquent et convaincant, je ne le laisse pas anesthésier mon esprit critique et ça ne m'empêche pas d'avoir une méthode rigoureuse pour rester responsable de tout ce que je produis.


## Conclusion

Après deux mois, le bilan est simple : il n'y a plus de retour en arrière possible. Pas parce que Claude Code est irremplaçable en théorie, mais parce qu'il a changé ce que je suis capable de faire en pratique. Évaluer un article en 30 secondes, avoir un correcteur qui a lu le même papier que moi et peut me confronter à ce que j'ai raté. Ce confort de travail, je ne pourrais plus m'en passer.

Les limites de l'attribution et de la responsabilité demandent seulement de la discipline. Le problème n'est pas l'IA, c'est l'utilisation qu'on en fait.

La vraie limite non résolue, c'est la confidentialité, et j'avoue que c'est une vraie épine dans le pied. Je rêverais d'avoir un modèle local aussi performant que Claude. On n'y est pas encore, mais je surveille ça avec attention.

Je ne doute pas que mon utilisation de Claude Code et mon opinion sur l'IA vont encore évoluer dans les mois à venir, je vous donne rendez-vous dans quelques mois pour une mise à jour ;)

---

## Annexe : prompts des skills

### Skill — Évaluation à priori d'un article scientifique

```markdown
---
description : évaluation à priori d'un article scientifique
disable-model-invocation: true
---

Lis Publications/`$ARGUMENTS`

Je cherche à savoir si ce papier vaut le coup d'être lu pour mon état de l'art.

**Mon sujet de thèse :** Caractériser et évaluer l'utilisation de Zero Trust et Data-Centric Security sur des systèmes OT critiques. Débordement possible sur de l'OT en Fédération et le Network Centric Warfare.

**Les axes que je dois couvrir :**
- Zero Trust : genèse et fondements conceptuels (périmètre, dé-périmétrage, least privilege, assume breach), modèles et architectures (PEP/PDP/PIP/PAP, SDP, SPA, microsegmentation, identity-centric), frameworks et standards (NIST, CSA, DOD, ANSSI, Forrester…), implémentations (BeyondCorp, ZTNA, IAM, mTLS…), limites et critiques (scalabilité, legacy, coût)
- Data-Centric Security : modèles d'autorisation (ABAC, MAC, ABE), labellisation, IRM, protection de la donnée en transit et au repos, Multi-Level Security
- Sécurité OT/ICS : architecture de référence (modèle de Purdue), protocoles industriels, contraintes spécifiques (temps réel, sûreté, patrimoine legacy), standards (IEC 62443, NIST SP 800-82), interconnexion IT/OT
- Network Centric Warfare : doctrine, infostructures militaires (DODIN, FMN, RM2SE…), Système de systèmes, Fédération, interopérabilité en coalition, contraintes de sécurité spécifiques au contexte opérationnel
- Cybermenaces : modèles d'attaque (Kill Chain, MITRE ATT&CK), acteurs étatiques, attaques sur OT (exemples concrets), vecteurs d'attaque liés à l'interconnexion IT/OT
- Sécurité réseau fondamentale : authentification (MFA, PKI, fédération d'identités), architecture réseau (micro-segmentation, SDN, VPN, NGFW), supervision (IDS, EDR, XDR, SIEM)

**Ce que j'ai déjà couvert :**

Les synthèses complètes sont dans `Concepts/Techniques/*/`. Résumé par axe :

**Zero Trust**
- Genèse et fondements : Jericho Forum (dé-périmétrage), Kindervag 2010, NIST SP 800-207
- Architecture : PEP/PDP/PIP/PAP, SDP, microsegmentation, identity-centric, ZTNA
- Frameworks lus : NIST SP 800-207, CSA SDP/ZT (2013/2019/2022/2025), DOD ZTA (2022/2025), ANSSI (2021/2025), Forrester
- Implémentations : BeyondCorp, Zscaler, Twingate, Trout, Access Proxy, Flank Speed (US Navy)

**Data-Centric Security**
- Modèles d'autorisation : DAC, MAC, IBAC/ACL, RBAC, ABAC, CapBAC, ReBAC, NGAC — avec leur orthogonalité axe gouvernance / axe décision
- ABE : principe d'intégration chiffrement+autorisation
- Multi-Level Security : modèle Bell-LaPadula (no read up / no write down), classifications hiérarchiques, compartiments
- Labellisation, Label binding (encapsulé/embarqué/détaché), IRM, SPIF, STANAG 4774/4778, ACP 240, Zero Trust Data Format
- Implémentations : NATO Metadata Binding Service, Microsoft Purview
- Concepts transversaux : Reference Monitor (Anderson 1972), Analog Hole
- Infrastructure de décision : XACML (formel), Open Policy Agent (pratique), PAP

**Authentification**
- Facteurs : MFA, OTP, FIDO2, authenticators (sais/as/est)
- Fédération : Credential Service Provider, Identity Provider, SAML, OpenID Connect, SSO, OAuth 2.0
- Machine-to-machine : mTLS, SPIFFE
- PKI : certificats X.509, chaîne de confiance, CA — utilisés dans mTLS, IPsec, FIDO2
- IAM : Active Directory (Kerberos), Okta
- Contexte réseau : 802.1X, EAP/EAPOL, RADIUS, NAC
- Avancé : Context Aware Authentication, Authentification continue, Authentification avant exposition réseau (SDP/SPA)

**Architecture réseau**
- Modèle périmétrique et ses limites (lateral movement, BYOD, cloud, télétravail)
- Micro-segmentation : Illumio, Guardicore (agent par endpoint, hyperviseur, conteneurs)
- SDN : OpenFlow, OpenDaylight
- Autres : VPN, NGFW, SD-WAN, MPLS, IPsec, IKE, Bastion, CASB, SASE, SWG

**Sécurité OT/ICS**
- Modèle de Purdue
- Contexte : Industry 4.0, interconnexion IT/OT
- Attaques concrètes lues : Industroyer (2016), Industroyer2 (2022), Triton (2017), BlackEnergy, Oldsmar, NotPetya/Maersk
- Standards mentionnés : IEC 62443, NIST SP 800-82 — mais peu approfondis

**Network Centric Warfare**
- Doctrine : DOD (1996–2001), Alberts 1999, NATO (2005), évolution vers CJADC2/Overmatch
- Infostructures US : DODIN, DISN, CANES, NGEN-R, ADNS, TSAT
- OTAN : AMN, FMN, Protected Core Networking
- Concepts : Système de systèmes, Fédération (vs centralisé/maillé/hiérarchique)

**Supervision**
- Méthodes de détection : signature, anomalie, comportement — et leurs limites (Axelsson 2000, Sommer 2010)
- Outils : IDS, EDR, XDR, SIEM, DLP, UAM, Antivirus, CTI, SWG
- Biais et limites : alert fatigue, base rate fallacy, gap sémantique, métriques de détection

Je te fournis le PDF complet (ou à défaut : titre, abstract, auteurs, revue, année, nombre de citations).

**Évalue selon ces 4 points :**
1. Pertinence par rapport à mon sujet de thèse
2. Apport nouveau par rapport à ce que j'ai déjà couvert
3. Qualité et crédibilité de la source
4. Verdict : **lire entièrement / extraire une section (laquelle) / lire en diagonale / ignorer** — justifié en une phrase
```

### Skill — Confrontation d'une note de lecture

```markdown
---
description: Donne un avis critique d'une note de lecture.
disable-model-invocation: true
---

Tu vas analyser une note de lecture et le PDF qu'elle commente.

## Étape 1 — Lire la note de lecture

La note s'appelle `$ARGUMENTS`
Lis le fichier `Reading notes/@$ARGUMENTS.md`.

## Étape 2 — Retrouver le PDF dans le .bib

Dans le fichier `.bib` à la racine, trouve l'entrée dont la clé correspond à `$ARGUMENTS` (ex: `JerichoForum2007`).

Extrais la valeur du champ `file`. Elle est au format `:nomfichier.pdf:PDF`.
Le nom du fichier est la partie entre les deux premiers `:`.

Le PDF se trouve à `Publications/<nomfichier.pdf>`.

## Étape 3 — Lire le PDF

Lis le PDF trouvé à l'étape 2.

## Étape 4 — Donner un avis critique

Donne un avis critique de la note de lecture.
```
