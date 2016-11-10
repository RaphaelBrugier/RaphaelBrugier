---
title: Wordpress login form with GWT and UiBinder
date: 2011-01-16T16:40:14Z
slug: "wordpress-login-form-with-gwt-and-uibinder"
aliases:
  - /blog/2011/01/wordpress-login-form-with-gwt-and-uibinder/
  - /posts/wordpress-login-form-with-gwt-and-uibinder/
disqus_identifier: "366559793"
categories: ["programming"]
tags: [GWT]
---

Following my previous post, I have decided to rewrite the example of the Wordpress login form using only UiBinder and the ClientBundle/Css features that comes with GWT. This example demonstrate how you can use only htmlPanel and UiBinder to build a perfect pixel Ui just with hmtl/css and no gwt panel.

You can [see the result there.]({{ site.baseurl }}assets/posts/wpLogin/WpLogin.html) Or you can [download and run the eclipse project from there]({{ site.baseurl }}assets/posts/wpLogin/wpLogin.zip).

To start I've just copied the html from my wordpress login page into the htmlPanel. Then I've renamed all the "id" on the divs into class, because gwt will only compile the css declared in the UiBinder into class css. I've replaced the &lt;form&gt; by a &lt;div&gt;. I've added some more styles and the image directly in the UiBinder, so GWT can compress and obfuscate it. In the application.css I've just added two styles for the body.

The main file that you want to see is WpForm.ui.xml :
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE ui:UiBinder SYSTEM "http://dl.google.com/gwt/DTD/xhtml.ent">
<ui:UiBinder xmlns:ui='urn:ui:com.google.gwt.uibinder'
	xmlns:g='urn:import:com.google.gwt.user.client.ui'>

	<ui:image field='logo' src='logo-login.gif' />

	<ui:style>
		.form {
			margin-left: 8px;
			padding: 16px 16px 40px 16px;
			font-weight: normal;
			-moz-border-radius: 11px;
			-khtml-border-radius: 11px;
			-webkit-border-radius: 11px;
			border-radius: 5px;
			background: #fff;
			border: 1px solid #e5e5e5;
			-moz-box-shadow: rgba(200, 200, 200, 1) 0 4px 18px;
			-webkit-box-shadow: rgba(200, 200, 200, 1) 0 4px 18px;
			-khtml-box-shadow: rgba(200, 200, 200, 1) 0 4px 18px;
			box-shadow: rgba(200, 200, 200, 1) 0 4px 18px;
			font: 11px "Lucida Grande", Verdana, Arial, "Bitstream Vera Sans",
				sans-serif;
		}
		
		.form .forgetmenot {
			font-weight: normal;
			float: left;
			margin-bottom: 0;
		}
		
		.button-primary {
			background: #2379a1;
			color: #ffffff;
			font-family: "Lucida Grande", Verdana, Arial, "Bitstream Vera Sans",
				sans-serif;
			font-weight: bold;
			text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.3);
			padding: 3px 10px;
			border: none;
			font-size: 12px;
			border-width: 1px;
			border-style: solid;
			-moz-border-radius: 11px;
			-khtml-border-radius: 11px;
			-webkit-border-radius: 11px;
			border-radius: 11px;
			cursor: pointer;
			text-decoration: none;
			margin-top: -3px;
		}
		
		.login .form p {
			margin-bottom: 0px;
		}
		
		@external gwt-Label;
		.login .gwt-Label {
			color: #777;
			font-size: 13px;
		}
		
		.form .forgetmenot .gwt-Label {
			font-size: 11px;
			line-height: 19px;
			color: #777;
			float: right;
			margin-left: 5px;
		}
		
		.form .submit {
			float: right;
		}
		
		.form p {
			margin-bottom: 24px;
		}
		
		@sprite .login h1 a {
			gwt-image: 'logo';
			width: 326px;
			height: 67px;
			text-indent: -9999px;
			overflow: hidden;
			padding-bottom: 15px;
			display: block;
		}
		
		.nav {
			text-shadow: rgba(255, 255, 255, 1) 0 1px 0;
			margin: 0 0 0 8px;
			padding: 16px;
		}
		
		.nav a {
			color: #21759B;
		}
		
		.input {
			font-size: 24px;
			width: 97%;
			padding: 3px;
			margin-top: 2px;
			margin-right: 6px;
			margin-bottom: 16px;
			border: 1px solid #e5e5e5;
			background: #fbfbfb;
			color: #555;
		}
		
		.backtoblog {
			position: absolute;
			top: 0;
			left: 0;
			border-bottom: #c6c6c6 1px solid;
			background: #d9d9d9;
			background: -moz-linear-gradient(bottom, #d7d7d7, #e4e4e4);
			background: -webkit-gradient(linear, left bottom, left top, from(#d7d7d7),
				to(#e4e4e4) );
			height: 30px;
			width: 100%;
		}
		
		.backtoblog a {
			text-decoration: none;
			display: block;
			padding: 8px 0 0 15px;
			color: #464646;
		}
		
		.login {
			width: 320px;
			margin: 7em auto;
			padding-top: 30px;
		}
	</ui:style>

	<g:HTMLPanel>
		<div class="{style.login}">
			<h1>
				<a title="Propulsé par WordPress" href="http://wordpress.org/">Le blog de Raph</a>
			</h1>
			<div class="{style.form}">
				<p>
					<g:Label>Login</g:Label>
					<br />
					<g:TextBox styleName="{style.input}" />
				</p>
				<p>
					<g:Label>Password</g:Label>
					<br />
					<g:TextBox styleName="{style.input}" />
				</p>
				<p class="{style.forgetmenot}">
					<g:CheckBox></g:CheckBox>
					<g:Label> Se souvenir de moi</g:Label>
				</p>
				<p class="{style.submit}">
					<g:Button styleName="{style.button-primary}">Connect</g:Button>

				</p>
			</div>

			<p class="{style.nav}">
				<a title="Récupération de mot de passe"
					href="http://{{ site.baseurl }}">Mot de passe oublié ?</a>
			</p>
		</div>
		<p class="{style.backtoblog}">
			<a title="Êtes-vous perdu(e)?" href="{{ site.baseurl }}">← Retour sur Le blog de
				Raph</a>
		</p>
	</g:HTMLPanel>
</ui:UiBinder>
~~~

WpLogin.css  :
~~~css
    * {
        margin: 0;
        padding: 0;
    }
    
    body {
        font: 11px "Lucida Grande", Verdana, Arial, "Bitstream Vera Sans",
            sans-serif !important;
            background-color: #f9f9f9 !important;
    }
~~~

And that's all !  So now, please don't follow the advices of writing all your html in a big static String and learn how to use the tool !

Happy coding !

[Result page]({{ site.baseurl }}assets/posts/wpLogin/WpLogin.html) Or you can [Download the eclipse project]({{ site.baseurl }}assets/posts/wpLogin/wpLogin.zip)