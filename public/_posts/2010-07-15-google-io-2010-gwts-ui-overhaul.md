---
layout: post
title: Google I/O 2010 – GWT’s UI overhaul
date: 2010-07-15 20:28:53.000000000 +02:00
redirect_from: "/blog/2010/07/google-io-2010-gwts-ui-overhaul/"
categories:
- GWT
tags:
- Google
- GWT
status: publish
type: post
published: true
meta:
  _twitterrelated_short_urlHash: 72eef056afca0fb3b019b89efb4bc413
  _twitterrelated_short_url: http://bit.ly/c6FKHE
  _edit_last: '1'
  _activeshortener: bitly
  dsq_thread_id: '371887625'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
Le 19 et 20 mai 2010 avait lieu l'événement annuel de Google, le Google I/O à San Francisco. Près d'une centaine de présentations ont eu lieux et toutes étaient filmées ! Je débute donc une série de post pour résumer celles que je visionne, en particulier celles traitant de GWT et de Google App Engine.

## GWT'S UI Overhaul

#### Par Ray Rian et Joel  Webber.

Cette présentation  présente plus en détails les nouvelles façons de construire les  interfaces  avec la version 2.0 de GWT et introduit une partie de la  future 2.1. L’équipe de GWT nomme ces nouveautés UI overhaul.

Elles sont  détaillées en 4 points :

*   UiBinder : La conception déclarative des  interfaces, qui permet de remplacer énormément de code java par un  fichier xml.
*   ClientBundle : Permet de lier les ressources de l’application au code java, ainsi GWT réalise des optimisations.
*   LayoutPanels : Présentation du nouveau système de rendu utilisant uniquement du html standard
*   CellWidgets (2.1) : Les futurs widgets pour construire des tableaux ou des listes avec de très nombreuses données

### UiBinder :

Le nouveau système de déclaration des interfaces avait été introduit avec Gwt 2.0, cette partie de la présentation rappelle seulement que UiBinder permet d'écrire beaucoup plus simplement les interfaces en supprimant le code inutile java. Pour l'utiliser dans mon projet, c'est un vrai bonheur !

Par exemple, voici du code java :

{% highlight java %}
    HorizontalPanel outer = new HorizontalPanel();
    VerticalPanel inner = new VerticalPanel();
    outer.setHorizontalAlignment(HorizontalPanel.ALIGN_RIGHT);
    inner.setHorizontalAlignment(HorizontalPanel.ALIGN_RIGHT);
    HorizontalPanel links = new HorizontalPanel();
    links.setSpacing(4);
    links.add(signOutLink);
    links.add(aboutLink);
    final Image logo = images.logo().createImage();
    outer.add(logo);
    outer.setCellHorizontalAlignment(logo, HorizontalPanel.ALIGN_LEFT);
    outer.add(inner);
    inner.add(new HTML("**Welcome back, foo@example.com**"));
    inner.add(links);
    signOutLink.addClickHandler(this);
    aboutLink.addClickHandler(this);
    initWidget(outer);
    setStyleName("mail-TopPanel");
    links.setStyleName("mail-TopPanelLinks");
{% endhighlight %}

Et son équivalent UiBInder :
{% highlight xml %}
<g:htmlpanel>
    <div class="{style.logo}">
        <div class="{style.statusDiv}">
            <div>
                **Welcome back, foo@example.com**
            </div>
            <div class="{style.linksDiv}">
                <g:anchor ui:field="signOutLink">Sign Out</g:anchor>
                <g:anchor ui:field="aboutLink">About</g:anchor>
            </div>
        </div>
    </div></g:htmlpanel>
{% endhighlight %}

### Client Bundle :

Une nouveauté au moins aussi important qu'UiBinder est le ClientBundle. L'interface ClientBundle permet de lier les  ressources de l’applications au code java. Ainsi GWT réalise des  optimisations. Par exemple les images peuvent être packagées et les  opérations de “sprites” directement réalisées.

ClientBundle permet aussi de déclarer les propriétés css dans une interface et un fichier distinct qui seront packagé par Gwt. Cela permet là encore d'optimiser, d'obfusquer les noms css pour éviter la collision et aussi d'ajouter quelques propriétés Css, comme les expressions ou les constantes.

De plus ClientBundle s'intègre parfaitement avec Uibinder et il est possible de déclarer une interface Css dans le fichier java dont l'implémentation est réalisée dans le fichier UiBinder.

### Internationalisation (I18n)

UiBinder supporte désormais mieux l'internationalisation des widgets. Grâce à la balise &lt;ui:msg&gt;, il n'est plus nécessaire de déclarer une interface Message. Un gros travail a été effectué sur la documentation pour refléter tout ces changements, cf la [documentation I18n](http://code.google.com/webtoolkit/doc/latest/DevGuideUiBinderI18n.html).

### Layout Panels

L’ancien système de  layout était, selon les dire même de Ray Cromwell, un cauchemar. Le  système était statique et construit à base de table imbriquées ! De plus cela ne  fonctionnait pas en “standards mode“ seulement en “quirks mode”. Le "standards mode" étant le futur du web, html5, il devenait critique de pouvoir l'utiliser et de se passer du vieux positionnement à base de tables.

L’alternative proposée  par les frameworks pour Gwt qui encapsulent des librairies Javascript,  type Gwt-ext, est souvent d’implémenter un nouveau système de layout. Ce  système totalement en Javascript rend souvent l’application très lente.

La solution retenue  pour le nouveau système est d’utiliser des bloc div en position absolu  et du css. En effet, les propriétés Css : top left right bottom width  height permettent de positionner très exactement un bloc à l’intérieur  d’un autre.

Voici par exemple le code UiBinder et son résultat html/css

{% highlight xml %}
     <g:docklayoutpanel unit="EM">
         <g:north size="15">
             <g:flowpanel>
                 <g:label>Header</g:label>
             </g:flowpanel>
         </g:north>

         <g:west size="15">
             <g:flowpanel>
                 <g:button>Page 1</g:button>
                 <g:button>Page 2</g:button>
             </g:flowpanel>
         </g:west>

         <g:center>
             <g:htmlpanel>some html</g:htmlpanel>
         </g:center>
     </g:docklayoutpanel>
{% endhighlight %}

{% highlight html %}
<div style="position: absolute; left: 0px; top: 0px; right: 0px; bottom: 0px;">
    <div style="position: absolute; overflow: hidden; left: 0em; top: 0em; right: 0em; height: 15em;">
        <div style="position: absolute; left: 0px; top: 0px; right: 0px; bottom: 0px;">
            <div class="gwt-Label">Header</div>
        </div>
    </div>

    <div style="position: absolute; overflow: hidden; left: 0em; top: 15em; bottom: 0em; width: 15em;">
        <div style="position: absolute; left: 0px; top: 0px; right: 0px; bottom: 0px;">
            <button type="button" tabindex="0" class="Gohuy86A">Page 1</button>
            <button type="button" tabindex="0" class="gwt-Button">Page 2</button>
        </div>
    </div>

    <div style="position: absolute; overflow: hidden; left: 15em; top: 15em; right: 0em; bottom: 0em;">
        <div style="position: absolute; left: 0px; top: 0px; right: 0px; bottom: 0px;">some html</div>
    </div>
</div>
{% endhighlight %}

On voit bien que les panels qui composent le dockLayout sont placés de façons statiques avec les propriétés css qui s'appliquent à un placement absolu.

##### En résumé

Ce que le nouveau système de layout permet :

*   Définir très exactement la structrure de l’interface de  l’application. Pour les widgets interne, on pourra continuer à les  définir en pur html/css classique.
*   Travailler en “standard mode“
*   Fonctionner sous IE6, (oui même le standards mode !)
*   Supporter des animations. Par exemple en effet de transition entre les changements de panels.

Attention! Le nouveau système de layout n'est pas :

*   Un remplacement total  pour le système placement HTML. Si vous utilisez déjà avec succès des  placements en HTML/CSS alors continuez avec !
*   Un système de layout  comme Swing/swt avec des “prefered size”. Ici le  système est plus simple  et les panels ne peuvent pas se redimensionner à  partir des tailles  “préférées“, c’est à dire négociée avec les panels  du même niveau.

### CellWidgets (2.1)

La prochaine version de GWT, la 2.1, introduira entre autres de nouveaux widgets pour afficher des listes/tableaux comportements de nombreuses données. La particularité de ces widgets est de favoriser le html pur pour accélérer le rendu. En effet, l'utilisation des widgets classiques pour afficher de grandes liste entrainait un surcout, à cause du grand nombre de fonction javascript que ces widgets possèdent. Cela menait souvent à diminuer la fluidité de la page dans le navigateur.

Les cellWidget sont un version simplifié du model MVC. La vue étend la classe Cell et possède une méthode onRender() qui retourne le html pour une cellule donnée en fonction du model qu'elle possède.

Il existe déjà une application de démonstrations qui tire parti de ces nouveaux widgets :

[Expenses application](http://gwt-bikeshed.appspot.com/Expenses.html)

[Le code source](http://www.google.com/url?sa=D&amp;q=http%3A%2F%2Fcode.google.com%2Fp%2Fgoogle-web-toolkit%2Fsource%2Fbrowse%2Fbranches%2F2.1%2Fbikeshed%2F%23bikeshed%2Fsrc%2Fcom%2Fgoogle%2Fgwt%2Fsample%2Fexpenses%2Fgwt)

### Conclusion :

Ma conclusion personnelle sur cette conférence est qu'il ne suffit pas d'utiliser GWT 2.0 pour compiler pour profiter de toute sa puissance. Comme toute toolbox, il faut prendre le temps de faire le tour de l'api pour la maîtriser.

### Sources :

[Page de la conférence](http://code.google.com/intl/fr/events/io/2010/sessions/gwt-ui-overhaul.html)

[Vidéos](http://www.youtube.com/watch?v=g2XclEOJdIc&feature=PlayList&p=F01F46882D8A90AF&playnext_from=PL&index=4)

[Slides](http://dl.google.com/googleio/2010/gwt-ui-overhaul.pdf)

[Wave de discussion sur la conférence](http://code.google.com/intl/fr/events/io/2010/sessions/gwt-ui-overhaul.html)

[Live Wave (notes prises par un Googler pendant la conférence)](https://wave.google.com/wave/waveref/googlewave.com/w+zqS7G7ZkBwa )