---
layout: page
ptitle: Software Packages
---
You can plot the dependencies of a package using:
{% highlight sh %}
apt install -y debtree &&
debtree --with-suggests curl > out.dot &&
dot -T png -o out.png out.dot
{% endhighlight %}