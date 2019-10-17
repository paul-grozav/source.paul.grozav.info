---
layout: page
ptitle: FFmpeg
---

[https://www.ffmpeg.org](https://www.ffmpeg.org)

### 1. Extract sub-video from second X to second Y.
The following example extracts from second `-ss 300`(5min) to second `-t 600`(10min). `-i` is the input file and `-qscale 0` tells it to keep the original quality. And the last parameter, is the output file.
{% highlight sh %}
ffmpeg -i original.mpg -ss 300 -t 600 -qscale 0 output.mpg
{% endhighlight %}

### 2. Concatenate multiple files into one.
{% highlight sh %}
# create file with list of all files to be concatenated
for f in [0-9].mpg; do echo "file ${f}" >> concat.txt; done &&
ffmpeg -f concat -i concat.txt -c copy output.mpg ;
# Remove temp file
rm -f concat.txt
{% endhighlight %}