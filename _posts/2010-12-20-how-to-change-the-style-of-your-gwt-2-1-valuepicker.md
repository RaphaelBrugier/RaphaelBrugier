---
layout: post
title: How-To change the style of your GWT 2.1 ValuePicker ?
date: 2010-12-20 21:59:06.000000000 +01:00
categories:
- GWT
tags:
- GWT
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  _activeshortener: bitly
  _wp_old_slug: ''
  _twitterrelated_short_url: http://bit.ly/gfRFUz
  _twitterrelated_short_urlHash: f56f401fc33a00c5f858badcf9a9f516
  dsq_thread_id: '366572829'
author:
  login: admin
  email: raphael.brugier@gmail.com
  display_name: Raphaël
  first_name: Raphaël
  last_name: ''
---
This class demonstrate how-to change the style of the GWT 2.1 ValuePicker.

{% highlight java %}
public class Main extends Composite {
	
	private static class DefaultCell<T> extends AbstractCell<T> {
	    private final Renderer<T> renderer;

	    DefaultCell(Renderer<T> renderer) {
	      this.renderer = renderer;
	    }

	    @Override
	    public void render(Context context, T value, SafeHtmlBuilder sb) {
	      sb.appendEscaped(renderer.render(value));
	    }
	  }
	
	public interface MyResources extends CellList.Resources {
	    @Source("myStyle.css")
	    Style cellListStyle();
	}
	
	Renderer<String> renderer = new AbstractRenderer<String>() {
		@Override
		public String render(String object) {
			return object;
		}
	};
	
	private static MyResources RESOURCES = GWT.create(MyResources.class);

	private static final Binder binder = GWT.create(Binder.class);
	
	@UiField FlowPanel panel;

	interface Binder extends UiBinder<Widget, Main> {
	}

	public Main() {
		initWidget(binder.createAndBindUi(this));

		CellList<String> cellList = new CellList<String>(new DefaultCell<String>(renderer), RESOURCES);

		ValuePicker<String> valueBox = new ValuePicker<String>(cellList);
		valueBox.setAcceptableValues(Arrays.asList("HELLO", "WORLD"));
		panel.add(valueBox);
	}
{% endhighlight %}