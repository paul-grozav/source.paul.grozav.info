---
layout: page
ptitle: FFmpeg
---

[https://www.ffmpeg.org](https://www.ffmpeg.org)

#### 1. Extract sub-video from second X to second Y.
The following example extracts from second `-ss 300`(5min) to second `-t 600`(10min). `-i` is the input file and `-qscale 0` tells it to keep the original quality. And the last parameter, is the output file.
{% highlight sh %}
ffmpeg -i original.mpg -ss 300 -t 600 -qscale 0 output.mpg
{% endhighlight %}
You can also use: `-ss 00:20:00 -t 00:05:00` to extract 5 minutes from minute 20.

#### 2. Concatenate multiple files into one.
{% highlight sh %}
# create file with list of all files to be concatenated
for f in [0-9].mpg; do echo "file ${f}" >> concat.txt; done &&

# Concatenate files into output file
ffmpeg -f concat -i concat.txt -c copy output.mpg ;

# Remove temp file
rm -f concat.txt
{% endhighlight %}

#### 3. Convert file
##### 3.1. `mpg` to `vob`
{% highlight sh %}
ffmpeg -i file.mpg -target pal-dvd -o file.vob
{% endhighlight %}
