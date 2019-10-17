## [FFmpeg](https://www.ffmpeg.org)


### 1. Extract sub-video from second X to second Y. 
The following example extracts from second `-ss 300`(5min) to second `-t 600`(10min). `-i` is the input file and `-qscale 0` tells it to keep the original quality. And the last parameter, is the output file.
{% highlight sh %}
ffmpeg -i original.mpg -ss 300 -t 600 -qscale 0 output.mpg
{% endhighlight %}