---
layout: post
title: Critique du Livre "Programmation GWT 2"
date: 2010-01-28 23:04:00.000000000 +01:00
redirect_from: "/blog/2010/01/critique-du-livre-programmation-gwt-2/"
categories:
- GWT
tags:
- GWT
status: publish
type: post
published: true
meta:
  _twitterrelated_short_urlHash: 0a4e5ba19d0459198d1c3af8c221d83c
  _twitterrelated_short_url: http://bit.ly/bNnYjX
  _thumbnail_id: '176'
  _edit_last: '1'
  _activeshortener: bitly
  dsq_thread_id: '366559766'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
Je viens de terminer la lecture de "Programmation GWT 2" de Sami Jabber. Sorti quelques jours après la release finale de cette nouvelle version majeure du framework, il constitue un excellent point d'entrée à cette technologie autant qu'un approfondissement pour les connaisseurs.

Bien que je connaisse déjà (un peu) GWT et que j'expérimente la version 2 depuis quelques mois, j'ai appris énormément avec ce livre. Et surtout j'ai pris beaucoup de plaisir à le lire! Les premiers chapitres constituent de bon rappel sur le fonctionnement général.

Le chapitre sur RPC est assez surprenant puisque l'auteur invite à utiliser la version 2 de ce service, [DeRPC (Direct-Eval RPC)](http://code.google.com/intl/fr/webtoolkit/doc/latest/DevGuideServerCommunication.html#DevGuideDeRPC) ... alors qu'il n'a pas été publiés en version finale et que toutes les classes sont marqués "WARNING EXPERIMENTAL DO NOT USE !". Personnellement je ne tenterais pas pour une production pro, mais c'est toujours intéressant de testé. Dans les tout cas, l'auteur propose [une justification](http://www.programmationgwt2.com/web/guest/chapitres/-/wiki/Main/Chapitre+7+-+Les+services+RPC).

Le chapitre sur J2EE propose un pattern vraiment très intéressant pour simplifier l'appel de services existant (spring, ejb) en évitant d'avoir à écrire des classes asynchrones.

Pour moi, le gros plus de ce livre réside dans le chapitre "Sous le capot". Il propose de plonger dans le coeur de GWT et décrit tout la mécanique de compilation JAVA vers JavaScript. C'est vraiment passionnant, et là on se rend bien compte de la prouesse technique qu'a nécessité la création de ce framework.

Les derniers chapitres exposent les nouveautés de la version 2 avec plus de pédagogie que les tutoriels du site officiel, un bon point.

Par contre, le chapitre sur les patterns est trop rapide à mon goût. Avec en plus la fin du chapitre dédié à la sécurité, je suis resté sur ma faim. Mais bon, ce n'est pas un livre sur les patterns et le lecteur pourra approfondir les patterns les plus intéressant par expérimentation par lui-même.

Un avis très positif donc pour ce livre. Le premier livre sur GWT 2 est en français et il est bon !

Nb : à noter qu'un [WIKI](http://www.programmationgwt2.com) est disponible pour corriger les coquilles du livre ou discuter avec l'auteur.