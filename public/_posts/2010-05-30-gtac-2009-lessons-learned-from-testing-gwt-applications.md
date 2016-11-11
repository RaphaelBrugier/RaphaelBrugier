---
layout: post
title: GTAC 2009 – Lessons learned from testing GWT applications
date: 2010-05-30 16:00:20.000000000 +02:00
redirect_from: "/blog/2010/05/gtac-2009-lessons-learned-from-testing-gwt-applications/"
categories:
- GWT
tags:
- GWT
status: publish
type: post
published: true
meta:
  _twitterrelated_short_urlHash: c2ab4184260a2c1b8108ee749e5e0f77
  _twitterrelated_short_url: http://bit.ly/cY9X4r
  _thumbnail_id: '257'
  _edit_last: '1'
  _activeshortener: bitly
  dsq_thread_id: '398993694'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
Les 21 et 22 octobre 2009 avait lieux à Zurich la conférence annuelle de Google sur les test automatisées.

Parmi toutes les conférences proposées, une m'intéressait particulièrement puisqu'elle concerne les test d'applications réalisées en Gwt.

Voici donc la vidéo de la présentation ainsi que les slides et quelques notes que j'ai pris en la visionnant.

<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" width="480" height="385" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="allowFullScreen" value="true" /><param name="allowscriptaccess" value="always" /><param name="src" value="http://www.youtube.com/v/TFfEjo3oFfM&amp;hl=fr_FR&amp;fs=1&amp;" /><param name="allowfullscreen" value="true" /><embed type="application/x-shockwave-flash" width="480" height="385" src="http://www.youtube.com/v/TFfEjo3oFfM&amp;hl=fr_FR&amp;fs=1&amp;" allowscriptaccess="always" allowfullscreen="true"></embed></object>


## Comment tester les applications Gwt ? (slide 8 )

###   Avec selenium :

Selenium va simuler au plus près l'interaction qu'a un vrai utilisateur avec le navigateur puisqu'il démarre un vrai navigateur et réalise directement les opérations de test sur celui-ci. Cependant cette approche est très coûteuse en temps d'exécution et surtout plus difficile pour maintenir les tests et plus dure à débugguer.

###   Avec GwtTestCase :

Cet outil, fourni avec Gwt ,démarre lui aussi un navigateur embarqué pour exécuter les tests. Cependant ici, on parle de "crossCompilation" puisque le code java est compilée en JS à la volée pour être exécuté par le navigateur embarqué. De la même façon que le development mode. Cet outil est plus rapide que selenium et plus facile à maintenir puisque les tests sont écrits en Java et lancés de la même façons que des tests Junit. Cependant, le fait que le code final testé est du javascript (après compilation à la volée donc) rend plus difficile le débuggage.

###   Avec Junit :

Ici on utilise classiquement Junit pour simuler les interactions utilisateurs. C'est le plus facile à maintenir et débugguer, puisque le mieux maîtriser et outillé. Cependant c'est aussi évidemment le moins réaliste puisqu'aucun navigateur n'est démarré pour tester le comportement réel du code compilé.

## Les problèmes rencontrés pendant les tests

### Problème 1 : tester les appels asynchrones (slides 9 à 13) :

Les appels aysnchrones sont la nature même des applications Ajax. C'est le A de Ajax ! Avec ces appels, il n'y a aucune garantie sur le temps nécessaire pour qu'ils réussissent, ni même d'ailleurs s'ils réussiront un jour (les pauvres :s). Ce sont ces mêmes appels que vous utilisez avec le mécanisme RPC de Gwt :

{% highlight java %} 
myservice.call(new AsyncCallBack() {
    public void onSuccess(QueryResponse response) {
        searchButton.setEnabled(true);

        if(response.count() == 0 {
            resultPanel.add(new Label("no team found");
        } else {
            //ajouter la réponse dans un panel, etc
        }
    }

    public void onFailure(Throwable ex) {
        //traiter l'échec de l'appel asynchrone
    }
}
{% endhighlight %}

Test :

{% highlight java %} 
MyService myService = mock(MyService.class);
QueryHandler client = new QueryHandler(mySerice);
client.performQuery("test query");
ArgumentCaptor&lt;Asyncallback&gt; captor = ArgumentCaptor.forClass(AsynCallback.class);
verify(myService).call("test query", captor.capture);
AsyncCallback&lt;QueryResponse&gt; callback = captor.getValue;

//Réel appel à onSuccess

QueryResponse fakeResponse = createFakeResponseForTesting();
callback.onSuccess(fakeResponse);

//Tester les résultats.
{% endhighlight %}

Ce que l'on veut tester ici, c'est le résultat d'un appel à la méthode onSuccess avec le résultat d'une réponse de callBack.

Ce qu'il faut bien voir c'est que toute la première partie du test ( mocker le service, mocker le callback, récupérer le callback de réponse), tout ça n'est fait que pour pouvoir appeler la méthode onSuccess du callBack ! ça fait donc beaucoup de plomberie pour tester une simple méthode !

L'astuce consiste à toujours remplacer le traitement qu'on ferait dans la méthode onSuccess par un appel à une autre méthode : doTraiterReponse(response);

Cette méthode, fera tout le travail de traitement nécessaire. Ainsi il est plus simple de la tester :

QueryHandler.java :

{% highlight java %} 
myservice.call(new AsyncCallBack() {
    public void  onSuccess(QueryResponse response) {
        doTraiterReponse(response);
    }

    public  void onFailure(Throwable ex) {
        //traiter l'échec de l'appel  asynchrone
    }
}
{% endhighlight %}

Test :

{% highlight java %} 
QueryHandler client = new QueryHandler(null) // Plus besoin de service !

QueryResponse fakeResponse = createFakeResponseForTesting();
client.doTraiterReponse(fakeresponse);

//Tester les résultats
{% endhighlight %}

### Problème 2 : Opérations directe sur le DOM (slides 14 à 16)

Gwt permet de manipuler directement le DOM du document HTML :

{% highlight java %}
String selectedLanguage = DOM.getElementByID("languageSelection").getPropertyString("value");
{% endhighlight %}

Ici, la solution est simple :APPRENDRE L'API !

En utilisant uniquement les widgets gwt qui encapsule tout la manipulation du DOM, il est plus facile de tester unitairement ces Widgets.

Ici on aurait utilisé une ListBox dans une panel.

### Problème 3 : mixer Java et Javascript (slides 17 à 19)

Gwt propose un mécanisme pour encapsuler du code javascript dans des méthodes Java :

{% highlight java %} 
private native void showPlayer /*{
    $wnd.showPlayer;
}*/;
{% endhighlight %}

Ici, le mélange javascript/java ne permet de tester unitairement avec Junit les méthodes natives.

La solution la plus simple reste d'isoler ces méthodes dans des classes dédiées et de les mocker quand elles sont utilisées.

Pour tester ces méthodes, il faudra faire appels à GwtTestCase qui lui aura la capacité d'exécuter du code mixant java (compilée à la volée en javascript) et appels natifs au code javascript.

### Problème 4 : static/global access (slides 20 à 27)

Les accès statics ou aux variables globales sont difficiles à suivre, et donc à débugguer.

Pour isoler les dépendances de la classe à tester, on mettra en oeuvre l'injection de dépendance pour mocker les classes utilisées dans les tests.

Pour rappel, il existe un framework d'injection de dépendances développé par Google pour Gwt : [Gin](http://code.google.com/p/google-gin/).

Il est basé sur Guice, le framework d'injection de Google.

### Problème 5 : Séparation des responsabilités (slides 28 à 32)

Ici le pattern MVP (Model View Presenter) est mis en avant par rapport au classique MVC.

Dans MVC, la vue est écouteur du modèle. Le contrôleur modifie le modèle et la vue écoute ces changements. La vue se met à jour en écoutant tout changement du modèle.

Cette mise à jour entraine forcément qu'une partie de la logique du traitement du modèle se retrouve dans la vue, et donc une mauvaise séparation des responsabilité.

Pour parer à cela, le pattern MVP à été introduit. L'article fondateur du pattern MVP est celui de [M. Fowler](http://martinfowler.com/eaaDev/ModelViewPresenter.html).

Dans MVP, toute la logique est concentrée dans le Presenter. Le Modèle et la Vue informe le Presenter des changements par un système d'évènements. Le Presenter réagit aux évènements et reflète les changements sur le modèle et la vue. Le plus souvent la vue sera d'ailleurs une vue passive.

Le slide 32 est particulièrement intéressant, puisque c'est lui qui ma donné le déclic pour comprendre toute la logique de MVP.

Avec un Presenter isolé de cette façons, il est facile de le tester unitairement (en pseudo code "java" pour plus de clarté) :

{% highlight java %} 
public class Presenter {
    Vue vue;
    Model model;

    public Presenter(Vue vue, Model model) {
        this.model = model;
        this.vue = vue;
        attachHandler();
    }

    private void attachHandler() {
        vue.getButton.addClickHandler(new ClickHandler() {
            public void onClick(Event e) {
                model.save(vue.getData());
            }
        });
    }
}
{% endhighlight %}

Test :
{% highlight java %}
//With
Model mockModel = mock(Model.class);
Vue mockVue = mock(Vue.class);
Presenter presenter = new Presenter(mockVue, mockModel);

//Given
Data testData = createFakeData();
vue.setData(testData);

// Assert
verify(mockModel).save(testData);
vue.getButton.click();
{% endhighlight %}

### Conclusion

Voilà pour les quelques notes sur cette conférence, il y'en a encore beaucoup d'autres à regarder qui ne porte pas que sur  GWT mais sur les Test et les bonnes pratiques en général.

Bon visionnage et surtout bon TDD à tous !