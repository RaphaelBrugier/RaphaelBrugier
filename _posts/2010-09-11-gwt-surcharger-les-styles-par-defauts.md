---
layout: post
title: Gwt – Surcharger les styles par défauts
date: 2010-09-11 12:55:17.000000000 +02:00
redirect_from: "/blog/2010/09/gwt-surcharger-les-styles-par-defauts/"
categories:
- GWT
tags:
- GWT
status: publish
type: post
published: true
meta:
  _twitterrelated_short_urlHash: e45803adf07a4de0b857712b20ecc4bd
  _twitterrelated_short_url: http://bit.ly/cxzQ7z
  _wp_old_slug: ''
  _edit_last: '1'
  _activeshortener: bitly
  dsq_thread_id: '367426457'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
La version 2.0 de Gwt à introduit l'interface CssResource qui permet de lier des classes css à des widgets Gwt en java. Cette interface permet de bénéficier d'optimisation à la compilation et de brouiller les noms des classes pour éviter les collisions. Elle enrichi aussi la syntaxe Css, par exemple en permettant de déclarer des constantes. Il est ainsi très facile de déclarer des feuilles Css qui joueront le rôle de feuille parent de laquelle hériterons les feuilles spécifiques à chaque partie de l'interface. Cela permet par exemple de créer très facilement plusieurs thèmes.

Cependant, il n'est parfois pas aisé de surchargé les styles du thème par défaut. En effet, l'interface les styles déclarés avec CssResources sont injectés uniquement quand nécessaire, et parfois après ceux de la feuille de thème par défaut qui les surchargent donc. Pour pallier à ce problème, il existe une astuce c'est d'utiliser l'annotation @External

{% highlight java %}
public interface WidgetCss extends CssResource {
        public void myButtonStyle();
}

public interface WidgetResources extends ClientBundle {
        public WidgetResources INSTANCE = GWT.create(WidgetResources.class);

        WidgetCss css();
}
{% endhighlight %}

Fichier css :

{% highlight css %}
@External .gwt-Button
.myButtonStyle .gwt-Button{
    color: #424242;
    font-size: 13px;
    cursor: pointer;
}
{% endhighlight %}

Utilisation :

{% highlight java %}
public MyWidget() {
    Button button = new Button("click me !");
    button.addStyleName(css().myButtonStyle());
}

/**
* @return Helper access to the css String;
*/
private WidgetResources css() {
    return WidgetResources.INSTANCE.css();
}
{% endhighlight %}

Et voilà, la déclaration du thème par défaut gwt-button est maintenant surchargée par myButtonStyle.

Le style ne sera pas surchargé par le thème par défaut, et il n'est pas nécessaire un moche hack avec par exemple "!important".

Bon gwt à tous !