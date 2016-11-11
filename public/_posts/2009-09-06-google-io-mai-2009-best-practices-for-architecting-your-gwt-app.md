---
layout: post
title: ! 'Google IO mai 2009 : Best Practices For Architecting Your GWT App'
date: 2009-09-06 14:34:00.000000000 +02:00
redirect_from: "/blog/2009/09/google-io-mai-2009-best-practices-for-architecting-your-gwt-app/"
categories:
- GWT
tags:
- GWT
status: publish
type: post
published: true
meta:
  _twitterrelated_short_url: http://bit.ly/bMVGvd
  _twitterrelated_short_urlHash: f2b51c0f0fc2c99919c9975e208e0098
  _edit_last: '1'
  _activeshortener: bitly
  dsq_thread_id: '368926929'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
Lors de la conférence annuelle de Google, en mai dernier, Ray Ryan à fait une présentation très intéressante sur les meilleures pratiques pour concevoir une application avec GWT. Cette présentation, a eu beaucoup d'écho dans la communauté GWT et plusieurs frameworks sont apparus pour faciliter l'implémentation de ces "bonne pratiques".

<object width="425" height="344"><param name="movie" value="http://www.youtube.com/v/PDuhR18-EdM&hl=fr&fs=1&" /><param name="allowFullScreen" value="true" /><param name="allowscriptaccess" value="always" /><embed src="http://www.youtube.com/v/PDuhR18-EdM&hl=fr&fs=1&" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="425" height="344"></embed></object>

    Voici un résumé sous forme de note de quelques points importants abordés dans cette présentation.

    #### AJAX

    *   Il faut minimiser la taille des objets retournés au client. Par exemple en ayant une liste d'identifiants d'objets en attribut plutôt qu'une liste d'objets.
    *   Il faut spécifier le type d'une liste lorsque cela est possible: 
    `ArrayList<Object> maListe;`

    plutôt que

    `List<Object> maListe;`

En effet, le compilateur GWT produira moins de code javascript s'il connaît plus précisément le type de liste.

#### Command Pattern

Le "Command Pattern" est un [pattern classique des IHM](http://dico.developpez.com/html/3161-Conception-Command-design-pattern-command.php).

Appliqué à GWT il permet de&nbsp;:

*   Mettre en place plus facilement une solution de cache. Avant d'appeler un service, la commande execute vérifiera qu'elle n'a pas déjà la donnée.
*   Mettre en place plus facilement des traitements par lots (batch).
*   Centraliser les messages d'erreurs. Les onFailure() des réponses asynchrones pourront directement être implémenté dans la classe command.
*   Découper le code javascript avec la méthode runAsync() de gwt 2.0  Les classes command qui encapsulent les services pourront être chargé par l'application que lorsque c'est nécessaire.

Les exemples de code présentés:

{% highlight java %}
/** Le nom Command est déjà utilisé dans GWT et est donc remplacé par Action */
interface Action<t extends response> { }

        interface Response { }

        interface ContactsService extends RemoteService {
        <t extends response> T execute(Action<t> action);
            }

            interface ContactsServiceAsync {
            void execute(Action<t> action, AsyncCallback<t> callback);
                }

                class GetDetails implements Action<getdetailsresponse> {
                    private final ArrayList<contactdetailid> ids;

                        public GetDetails(ArrayList<contactdetailid> ids) {
                            this.ids = ids;
                            }

                            public ArrayList<contactdetailid> getIds() {
                                return ids;
                                }
                                }

                                class GetDetailsResponse implements Response {
                                private final ArrayList<contactdetail> details;
                                    public GetDetailsResponse(ArrayList<contactdetail> details) {
                                        this.details = details;
                                        }
                                        public ArrayList<contactdetail> getDetails() {
                                            return new ArrayList<contactdetail>(details);
                                                }
                                                }

                                                abstract class GotDetails implements
                                                AsyncCallback<getdetailsresponse> {
                                                    public void onFailure(Throwable oops) {
                                                    /* default appwide failure handling */
                                                    }
                                                    public void onSuccess(GetDetailsResponse result) {
                                                    got(result.getDetails());
                                                    }
                                                    public abstract void got(ArrayList<contactdetail> details);
                                                        }

                                                        void showContact(final Contact contact) {
                                                        service.execute(new GetDetails(contact.getDetailIds()),
                                                        new GotDetails() {
                                                        public void got(ArrayList<contactdetail> details) {
                                                            renderContact(contact);
                                                            renderDetails(details);
                                                            }
                                                            });
                                                            }</contactdetail></contactdetail></getdetailsresponse></contactdetail></contactdetail></contactdetail></contactdetail></contactdetailid></contactdetailid></contactdetailid></getdetailsresponse></t></t></t></t></t>
{% endhighlight %}

        
        
Une librairie proposant une implémentation du command-pattern est disponible : [gwt-dispatch](http://code.google.com/p/gwt-dispatch/)

#### Event bus

Plutôt que d'enregistrer les composants graphiques entre eux et d'utiliser le classique _MVC_, Ray recommande d'utiliser un "Event bus". Chaque composant s'enregistre sur le bus (en fait un handlerManager) et reste à l'écoute d'événements.

### Model-View-Presenter (MVP)

Le "[Model-View-Presenter](http://martinfowler.com/eaaDev/SupervisingPresenter.html)" pattern vient en remplacement du classique _MVC_ dans les applications GWT.

L'utilisation de ce pattern facilite les Test Unitaires, seul la partie "Presenter" a besoin d'être testée. Les changements dans la partie "View" sont entièrement géré par la partie "Presenter".

Une librairie proposant une implémentation du MVP et de l'event-bus est disponible : [gwt-presenter](http://code.google.com/p/gwt-presenter/)

#### Injection de dépendance

L'injection de dépendance est réalisé du coté client avec le framework GIN et du coté serveur avec le framework GUICE. Je ne pense pas que ce soit nécessaire de rappeler les bénéfices de l'injection, Spring est là pour ça ;)

#### Test unitaires

La partie "Presenter" du pattern MVP est facilement testable grace à l'utilisation de mock pour simuler la partie "View". EasyMock ou d'autres frameworks simplifie la création de mock et des test unitaires.