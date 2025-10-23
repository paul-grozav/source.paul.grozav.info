---
layout: page
title: asciinema
---

[https://asciinema.org](https://asciinema.org)
{% highlight sh %}
pip install --user asciinema
asciinema rec demo.cast
asciinema play demo.cast
{% endhighlight %}

If you want to play it in a browser, you can create the `asciinema_player.html`
file with the following content inside it:
{% highlight html %}
<!-- ----------------------------------------------------------------------- -->
<!-- Authors:                                                                -->
<!-- - Tancredi-Paul Grozav <paul@grozav.info>                               -->
<!-- ----------------------------------------------------------------------- -->
<!-- Update the *.cast file name, then serve it with the following command:  -->
<!-- python3 -m http.server                                                  -->
<!-- Then open this url in your browser:                                     -->
<!-- http://127.0.0.1:8000/asciinema_player.html                             -->
<!-- ----------------------------------------------------------------------- -->
<!DOCTYPE html>
<html>
<head>
  <link href="https://cdn.jsdelivr.net/npm/asciinema-player@3.8.0/dist/bundle/asciinema-player.min.css" rel="stylesheet">
</head>
<body>
  <div id="demo"></div>
  <script src="https://cdn.jsdelivr.net/npm/asciinema-player@3.8.0/dist/bundle/asciinema-player.min.js"></script>
  <script>
    AsciinemaPlayer.create('./test.cast', document.getElementById('demo'));
  </script>
</body>
</html>
<!-- ----------------------------------------------------------------------- -->
{% endhighlight %}
Save this html file next to your cast(`test.cast` in this case), and run:
`python3 -m http.server` to serve it as a web page, then go to:
[http://127.0.0.1:8000/asciinema_player.html](
  http://127.0.0.1:8000/asciinema_player.html) to play the recording.
