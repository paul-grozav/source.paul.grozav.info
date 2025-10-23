---
layout: page
title: Numeric limits
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML" async="" type="text/javascript">// <![CDATA[
// ]]></script>
<table border="1px">
<thead>
<tr>
<td><strong>Type</strong></td>
<td><strong>Bits used</strong></td>
<td><strong>Bytes used</strong></td>
<td><strong>Min value</strong></td>
<td><strong>Max value</strong></td>
<td><center><strong>Min formula</strong></center></td>
<td><center><strong>Max formula</strong></center></td>
</tr>
</thead>
<tbody>
<tr>
<td><strong>char</strong></td>
<td>8</td>
<td>1</td>
<td>-128</td>
<td>127</td>
<td style="text-align: center;">$$-(2^{7})$$</td>
<td>$$2^{7}-1$$</td>
</tr>
<tr>
<td><strong>unsigned char</strong></td>
<td>8</td>
<td>1</td>
<td>0</td>
<td>255</td>
<td>$$0$$</td>
<td>$$2^{8}-1$$</td>
</tr>
<tr>
<td><strong>short int</strong></td>
<td>16</td>
<td>2</td>
<td>-32768</td>
<td>32767</td>
<td>$$-(2^{15})$$</td>
<td>$$2^{15}-1$$</td>
</tr>
<tr>
<td><strong>unsigned short int</strong></td>
<td>16</td>
<td>2</td>
<td>0</td>
<td>65535</td>
<td>$$0$$</td>
<td>$$2^{16}-1$$</td>
</tr>
<tr>
<td><strong>int</strong></td>
<td>32</td>
<td>4</td>
<td>-2147483648</td>
<td>2147483647</td>
<td>$$-(2^{31})$$</td>
<td>$$2^{31}-1$$</td>
</tr>
<tr>
<td><strong>unsigned int</strong></td>
<td>32</td>
<td>4</td>
<td>0</td>
<td>4294967295</td>
<td>$$0$$</td>
<td>$$2^{32}-1$$</td>
</tr>
<tr>
<td><strong>long long int</strong></td>
<td>64</td>
<td>8</td>
<td>-9223372036854775808</td>
<td>9223372036854775807</td>
<td>$$-(2^{63})$$</td>
<td>$$2^{63}-1$$</td>
</tr>
<tr>
<td><strong>unsigned long long int</strong></td>
<td>64</td>
<td>8</td>
<td>0</td>
<td>18446744073709551615</td>
<td>$$0$$</td>
<td>$$2^{64}-1$$</td>
</tr>
</tbody>
</table>
<p>The formulas are (where (n) is the number of bits):</p>
<table border="1px">
<tbody>
<tr>
<td></td>
<td style="text-align: center;"><strong>min</strong></td>
<td style="text-align: center;"><strong>max</strong></td>
</tr>
<tr>
<td><strong>signed</strong></td>
<td><span style="font-size: 20px;">$$-(2^{n-1})$$</span></td>
<td><span style="font-size: 20px;">$$(2^{n-1})-1$$</span></td>
</tr>
<tr>
<td><strong>unsigned </strong></td>
<td><span style="font-size: 20px;">$$0$$</span></td>
<td><span style="font-size: 20px;">$$2^n -1$$</span></td>
</tr>
</tbody>
</table>
We could use: <a href="http://en.cppreference.com/w/cpp/types/integer" target="_blank">http://en.cppreference.com/w/cpp/types/integer</a>.
