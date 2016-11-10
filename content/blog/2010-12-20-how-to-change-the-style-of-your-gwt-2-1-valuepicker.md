---
title: How-To change the style of your GWT 2.1 ValuePicker ?
date: 2010-12-20T21:59:06Z
slug: "how-to-change-the-style-of-your-gwt-2-1-valuepicker"
disqus_identifier: "366572829"
aliases:
  - /blog/2010/12/how-to-change-the-style-of-your-gwt-2-1-valuepicker/
  - /posts/how-to-change-the-style-of-your-gwt-2-1-valuepicker/
categories: ["programming"]
tags: ["GWT"] 
---

This class demonstrate how-to change the style of the GWT 2.1 ValuePicker.

~~~java
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
~~~