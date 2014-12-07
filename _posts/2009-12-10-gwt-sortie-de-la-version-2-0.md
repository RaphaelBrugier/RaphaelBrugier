---
layout: post
title: GWT Sortie de la version 2.0
date: 2009-12-10 21:51:00.000000000 +01:00
categories:
- GWT
tags:
- GWT
status: publish
type: post
published: true
meta:
  _twitterrelated_short_url: http://bit.ly/cqS9uJ
  _twitterrelated_short_urlHash: 4a1c04f6441f2c9846279101bdc74a23
  _edit_last: '1'
  _activeshortener: bitly
  dsq_thread_id: '408829762'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
<p>Il y a quelques jours, Google a annoncé la sortie de la version 2 de son framework GWT.<br />
Cette nouvelle version majeur marque un tournant en apportant de nombreuses fonctionnalités destinées à faciliter la vie du développeur.<br />
L'accent à clairement été mis sur la rapidité dans cette nouvelle version, rapidité pour le développement et rapidité pour l'utilisateur final</p>
<h3>UI Binder</h3>
<p>L'UI Binder tout d'abord est une nouvelle façon de décrire les interfaces GWT de façons déclarative avec une syntaxe XML. L'utilisation d'UI Binder permet de séparer clairement l'interface de la logique l'application. Par la suite il sera donc facile de modifier l'interface de l'application. UI Binder à été complètement intégré au plugin google pour Eclipse, avec par exemple un wizard de création, la complétion du code, le refactoring, ...<br />
Un petit exemple pour comparer une interface écrite "à la" swing et avec UI Binder :</p>
<p>En java :</p>
<pre name="code" class="java">final Label label = new Label("Enter your name");
final Button sendButton = new Button("Send");
HorizontalPanel hPanel = new HorizontalPanel();
hPanel.add(label);
VerticalPanel vPanel = new VerticalPanel();
vPanel.add(hPanel);
vPanel.add(button);</pre>
<p>Avec Ui binder :</p>
<pre name="code" class="xml" 
<g:verticalpanel>
	<g:horizontalpanel>
		<g:label>Enter your name</g:label>
	</g:horizontalpanel>
	<g:button text="Send" stylename="{style.pretty}" ui:field="button" />
</g:verticalpanel>

<h3>Development mode</h3>
Sûrement la fonctionnalité qui va faire gagner le plus de temps aux développeurs. Exit le hosted mode, bonjour le development mode ! :)
Désormais, un simple plugin installé dans le browser remplace le navigateur de test qui était fournit. Le principe est toujours le même, une simple sauvegarde du fichier modifié et un refresh dans le navigateur permettent de voir les changements. Inutile de compiler en javascript pendant de longues minutes, le byte code java est utilisé à la place. Ceci permet donc par ailleurs de pouvoir tester en même temps le rendu dans plusieurs navigateurs.
<h3>Amélioration du code compilé.</h3>
Comme à chaque nouvelle version, la qualité du code javascript produit par le compilateur est encore optimisée. Le simple fait donc de recompiler un projet avec cette nouvelle version devrait donc sensiblement améliorer les performances de votre applications web.
<h3>Code Splitting</h3>
Le code splitting part d'un constat simple : pourquoi télécharger la totalité du code javascript de l'application quand on pourrait télécharger à la demande à la manière du streaming ? Les ingénieurs GWT ont donc ajouté cette fonctionnalité et le premier bénéficiaire en est Wave qui a vu son temps de téléchargement initial considérablement diminué.
Le principe : différer le téléchargement des blocs indépendants de l'application. Quelques lignes de codes suffisent pour cela :
<pre name="code" class="java">
GWT.runAsync(new RunAsyncCallback() {
	public void onFailure(Throwable caught) {
		Window.alert("Code download failed");
	}             

	public void onSuccess() {
             Window.alert("Hello, AJAX");           
	}         
});
</pre>
<h3>Speed Tracer</h3>
<p>Un outil de profiling a été annoncé en même temps que cette nouvelle version de GWT. Speed tracer est en fait une extension pour Google Chrome qui permet de profiler les requêtes, les temps de parsing et les temps de rendus du navigateur. Speed tracer identifie automatiquement les problèmes de performances de votre application.<br />
<a href="/public/images/tutos/speedTracer.png"><img style="display: block; margin: 0 auto;" title="speedTracer.png, déc. 2009" src="assets/.speedTracer_m.jpg" alt="speedTracer.png" /></a></p>
<h3>Conclusion</h3>
<p>Avec cette nouvelle version de GWT, Google continue la guerre des RIA. L'ensemble des fonctionnalités permettra de diminuer le temps de développement et d'améliorer l'expérience utilisateur.<br />
Et bien sur, tout cela repose toujours sur les standards HTML 5 + CSS + Ajax donc inutile d'installer un plugin propriétaire dans le navigateur comme le demandent flash et silverlight.<br />
Le premier livre sur  GWT 2 devrait sortir très prochainement et la bonne nouvelle c'est qu'il est en français !<br />
C'est écrit par <a href="http://www.dng-consulting.com/blogs/">Sami Jabber</a> et vous pouvez déjà <a href="http://www.eyrolles.com/Accueil/Livre/programmation-gwt-2-9782212125696">lire le sommaire et un chapitre entier</a> ou <a href="http://www.amazon.fr/Programmation-Concevoir-D%C3%A9velopper-Applications-Toolkit/dp/2212125690/ref=pd_sxp_grid_pt_0_0">le commander ici</a>.</p>
<h4>Sources :</h4>
<p>Google camp fire on GWT and Speed Tracer : <a href="http://www.youtube.com/watch?v=D2ibM4oufdM">part 1</a>, <a href="http://www.youtube.com/watch?v=JQpuDB2Jxfg&amp;feature=channel">part 2</a>, <a href="http://www.youtube.com/watch?v=QrnbKZ3hxls&amp;feature=channel">part 3</a>, <a href="http://www.youtube.com/watch?v=WM6KPW8ZyjU&amp;feature=channel">part 4</a>, <a href="http://www.youtube.com/watch?v=mnSNVfxK19Y&amp;feature=channel">part 5</a>, <a href="http://www.youtube.com/watch?v=Kfh6IX-yhsc&amp;feature=channel">part 6</a></p>
