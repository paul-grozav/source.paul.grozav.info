---
layout: page
ptitle: Network hardware and software discovery
---

Try to detect hardware equipments/nodes from your network and the software running the computer.
### 1. arp-scan
{% highlight sh %}
$ docker run -it --rm=true --privileged --net=host debian:10.1 bash
# apt update && apt install -y arp-scan && /usr/sbin/arp-scan -xl
{% endhighlight %}
will return something like:
{% highlight sh %}
192.168.0.1    64:70:02:e8:95:f0	TP-LINK TECHNOLOGIES CO.,LTD.
192.168.0.4    4c:bd:8f:77:de:ae	D-Link International
192.168.0.5    f8:3f:51:16:7a:5d	GIGA-BYTE TECHNOLOGY CO.,LTD.
192.168.0.6    7c:49:eb:a0:e3:37	(Unknown)
192.168.0.11   0e:80:62:bc:04:24	ASUSTek COMPUTER INC.
{% endhighlight %}

