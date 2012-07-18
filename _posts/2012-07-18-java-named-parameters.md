---
layout: post
category : blog
title: HOWTO Java named parameters
tags : [dev, java, lol]
---
{% include JB/setup %}

{% highlight java %}
class Request { }
class ManagerProvider { }

class Job {
	Job(ManagerProvider provider, Request request, OutputStream out) { }
}

class JobPerformer {

	void elite() {
		OutputStream output = ...;
		Job job = new Job(new ManagerProvider(), request = new Request(), out = output); // HERE !
	}

	private Object request;
	private Object out;
}
{% endhighlight %}

 =D
