---
layout: page
ptitle: PDF manipulation tools
---

[https://en.wikipedia.org/wiki/PDF](https://en.wikipedia.org/wiki/PDF)

{% highlight sh %}
# unencrypt .pdf file
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dNOPAUSE -dQUIET -dBATCH -sOutputFile=./my_unencrypted_output.pdf ./my.pdf

# Extract pages 1 to 13 into separate files - or https://www.ilovepdf.com/split_pdf
pdfseparate -f 1 -l 13 ./my.pdf ./tmp_page_%d.pdf

# Unite all *.pdf files into one - or https://www.ilovepdf.com/merge_pdf
pdfunite $(ls -la ./tmp_page_*.pdf | awk '{print $9}' | sort -V) ./my_output.pdf && rm -f ./tmp_page_*.pdf

# Compress resulting PDF - this produces better results, by far: https://www.ilovepdf.com/compress_pdf
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/prepress -dNOPAUSE -dQUIET -dBATCH -sOutputFile=my_compressed_output.pdf my_input.pdf
{% endhighlight %}

<style>
  ol { counter-reset: item }
  ol li { display: block }
  ol li:before { content: counters(item, ".") ". "; counter-increment: item }
</style>

Or, online services that can do this:
<ol>
  <li><a href="https://www.ilovepdf.com/" target="_blank">ilovepdf.com</a> - split, merge, convert, etc.
    <ol>
      <li><a href="https://www.ilovepdf.com/compress_pdf" target="_blank">compress_pdf</a> - compress (even embedded images are reduced in quality)</li>
    </ol>
  </li>
  <li><a href="https://pdfcompressor.com/" target="_blank">pdfcompressor.com</a> (can not compress as good as ilovepdf.com)</li>
</ol>
