---
layout: page
ptitle: PDF manipulation tools
---

[https://en.wikipedia.org/wiki/PDF](https://en.wikipedia.org/wiki/PDF)

{% highlight sh %}
# unencrypt .pdf file
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dNOPAUSE -dQUIET -dBATCH -sOutputFile=./my_unencrypted_output.pdf ./my.pdf

# Extract pages 1 to 13 into separate files
pdfseparate -f 1 -l 13 ./my.pdf ./tmp_page_%d.pdf

# Unite all *.pdf files into one
pdfunite $(ls -la ./tmp_page_*.pdf | awk '{print $9}' | sort -V) ./my_output.pdf && rm -f ./tmp_page_*.pdf

# Compress resulting PDF
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/prepress -dNOPAUSE -dQUIET -dBATCH -sOutputFile=my_compressed_output.pdf my_input.pdf
{% endhighlight %}
