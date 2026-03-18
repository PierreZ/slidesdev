---
source: https://gist.github.com/bb4055ca0912cc0bb976f3cb9c12bf71
created: 2026-02-23
updated: 2026-02-23
tags: [conferences, simulation, foundationdb, cfp]
---

# Devoxx France 2025 - Deterministic Simulation

## devoxx.2025.md

### Titles

- A la decouverte du test par simulation deterministe
- Et si on faisait du simulation-driven development ?

### Abstract

Avez-vous deja rencontre LE fameux bug ? Celui qui survient lorsque des pannes et erreurs improbables s'enchainent de maniere imprevue ? Ces problemes, courants dans les systemes distribues, echappent souvent aux tests classiques car il est impossible de reproduire fidelement les conditions ayant conduit a l'erreur.

Et si vous pouviez ? C'est ce que permet le Deterministic Simulation Testing (DST), en controlant precisement le temps, les latences, les pannes et les erreurs, il permet de simuler, rejouer et diagnostiquer les scenarios les plus complexes de maniere deterministe.

Dans ce talk, nous explorerons les limites des tests classiques, les concepts cles du DST et ses avantages concrets. Nous partagerons egalement comment Clever Cloud utilise cette methode pour developper et fiabiliser son offre de bases de donnees serverless.

### Mot pour la conference

Je vous propose cette annee un talk sur une approche innovante que nous employons chez Clever Cloud : **le test par simulation deterministe**, une methode puissante pour ameliorer a la fois la fiabilite et l'experience des developpeurs des systemes complexes.

J'ai la chance de pouvoir travailler autour des systemes distribues chez des fournisseurs de cloud depuis plus de 7 ans. Durant toutes ces annees, j'ai toujours travaille autour des logiciels d'infrastructures, comme Hadoop, Hbase, Kafka, Zookeeper, Pulsar, ETCD, OpenStack, Kubernetes, FoundationDB. A chaque fois, j'ai eu la chance de pouvoir a la fois operer la technologie en etant d'astreinte, et de pouvoir decouvrir les internals en allant lire le code et le patcher si necessaire.

Un element recurrent dans ces experiences : la decouverte de bugs rares. Ces erreurs, souvent difficiles a reproduire, m'ont pousse a remettre en question les limites des tests traditionnels (unitaires, d'integration, ou meme end-to-end). Ces derniers sont utiles pour eviter les regressions de features, mais ils echouent a capturer les combinaisons inattendues d'evenements propres aux systemes distribues.

Chez Clever Cloud, nous avons adopte une approche plus systematique : la **simulation deterministe**. L'idee est simple : generer automatiquement un large eventail de scenarios en controlant chaque aspect du systeme (I/O, scheduling des threads, pannes aleatoires, etc.). Chaque simulation est reproductible grace a une seed aleatoire qui capture precisement l'etat initial et les interactions. Lorsqu'un bug apparait, il suffit de rejouer cette seed pour obtenir une trace complete et deterministe de l'execution ayant cause l'erreur.

Chercher des bugs correspond a faire tourner des milliers de simulation par heure. Quand l'on trouve une seed qui nous donne un faux resultat, le developpeur n'a plus qu'a rejouer la seed deterministe pour venir reproduire tout le contexte. Cela donne egalement enormement de confiance a nos developpeurs lors de la mise en production.

Le but de ce talk est de vous parler de la simulation, de ses bienfaits et de l'impact que ca peut avoir sur le developpement d'un produit ou d'un logiciel complexe. On parlera egalement des contributions open-sources que nous avons faites dans ce domaine, notamment aupres de FoundationDB.

---

## sunny.2026.md

### Titles

- A la decouverte du test par simulation deterministe
- Et si on faisait du simulation-driven development ?

### Abstract

Avez-vous deja rencontre LE fameux bug ? Celui qui survient lorsque des pannes et erreurs improbables s'enchainent de maniere imprevue ? Ces problemes, courants dans les systemes distribues, echappent souvent aux tests classiques car il est impossible de reproduire fidelement les conditions ayant conduit a l'erreur.

Et si vous pouviez ? C'est ce que permet le Deterministic Simulation Testing (DST), en controlant precisement le temps, les latences, les pannes et les erreurs, il permet de simuler, rejouer et diagnostiquer les scenarios les plus complexes de maniere deterministe.

Dans ce talk, nous explorerons les limites des tests classiques, les concepts cles du DST et ses avantages concrets. Nous partagerons egalement comment Clever Cloud utilise cette methode pour developper et fiabiliser son offre de bases de donnees serverless.

### Mot pour la conference

Ce talk a ete presente a Devoxx France 2025 (https://www.youtube.com/watch?v=12LO_l90vDk&t=1031s). Le contenu sera assez similaire, mais avec un focus supplementaire sur l'angle LLM vers la fin du talk.

Depuis le talk a Devoxx France, les choses ont bien avance. L'integralite de l'equipe Materia utilise desormais la simulation comme un veritable outil de developpement au quotidien, ce n'est plus une experimentation mais une partie integrante de notre workflow.

En parallele, j'ai developpe mon propre simulateur de systemes distribues en Rust, ce qui m'a permis d'approfondir encore plus les concepts et de pousser l'exploration au-dela du cadre professionnel.

Enfin, chez Clever Cloud, nous envisageons de mettre la vitesse superieure sur la simulation comme outil pour pallier les problemes de qualite du code genere par IA. La simulation deterministe offre un filet de securite ideal : plutot que de faire confiance aveuglement au code produit par un LLM, on le soumet a des milliers de scenarios de pannes pour decouvrir activement les bugs qu'il pourrait introduire. C'est ce shift de paradigme -- passer de la prevention a la decouverte -- que je souhaite partager en fin de talk.
