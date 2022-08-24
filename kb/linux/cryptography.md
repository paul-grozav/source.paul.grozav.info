---
layout: page
ptitle: Cryptography
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML" async="" type="text/javascript">// <![CDATA[
// ]]></script>


## 0. Introduction
Symmetric encryption is an old technique, that relies on a shared "key" between conversation participants. For example the [Caesar Cipher](https://en.wikipedia.org/wiki/Caesar_cipher) can be considered a shared "key". An agreement(key) that both the emitter and the receiver have to know, in order to be able to encrypt and decrypt the message using this procedure(key). Or you could say that the Caesar Cipher is the encryption algorithm, and the key is "shift right by 3".

Symmetric encryption is efficient in terms of computation, but can only be secure if the conversation participants have an initial way of talking (in private) and agreeing upon the key. If the key is transmitted in plain text / over a public channel, a man in the middle attack could happen. That is an eavesdropper could obtain the key and use it to further decrypt all supposedly private conversation.

A more secure way of encrypting, is by using an asymmetric key. The RSA algorithm for encrypting and decrypting messages based on asymmetric keys, was discovered in 1977 and is widely used these days because it does not require the conversation participants to physically meet in order to exchange a secret shared key. So, RSA can ensure secure communication over insecure channels between any two participants.

## 0.1. Simple math example
So, let's take a mathematical example (based on [this video](https://www.youtube.com/watch?v=UjIPMJd6Xks)).

1. **Bob** wants to send a secret message to **Alice**. This can be a number, say **m=89**. But how can he send his message over a public, insecure communication channel, where **Eve** is always eavesdropping/listening. Eve will receive/see everthing that Alice and Bob communicate.
2. **Alice** will start by generating her key **pair**, consisting of a **private key** and a **public key**. For this, she picks two random large prime numbers, of similar size, say (we'll keep them small for this example) **p1=53** and **p2=59**. Then she multiplies them together `p1 * p2 = 53 * 59` = **3127 = n** . Then, Alice will calculate $f(p1, p2) = (p1 - 1) \cdot (p2 - 1) = f_n$ to be `(53-1) * (59-1) = 52 * 58` = **3016 = f_n**. Next, Alice picks a small number **e** that it is an odd number and does not share a factor with **f_n**. For example **e = 3** . Finally, Alice will calculate $d = \dfrac{2 \cdot f_n + 1}{e}$ , which in this case `d = (2 * 3016 + 1)/3` = **2011 = d**.
3. At this point Alice has her public key consisting of numbers **n**(=3127) and **e**(=3).
4. Alice also has her private key, consisting of the number **d** (=2011)
5. Alice sends the public key to Bob (over an insecure communication channel), to secure his message.
6. Bob will encrypt the message(m=89) by calculating **c**`=m^e mod n` that is `89^3 mod 3127 =` **1394 = c**, his encrypted message, which he sends back to Alice.
7. Alice can decrypt the encrypted message **c=1394** by applying the private key, **d**, to it. That is `c^d mod n` will be equal to `m`, the original message of Bob. Doing the math it is `1394^2011 mod 3127 =` **89 = m**.

Note that **Eve** which only has Alice's public key ( that is **n=3127** and **e=3** ) and Bob's encrypted message **c=1394** can not decrypt the message, because it would need **d** which depends on **f_n** which she doesn't have. And **f_n** is hard to guess because it requires Eve to guess the numbers **p1** and **p2** which are large, and are the factors of **n**.

This whole trick relies on the formula `n = p1 * p2` - the fact that prime factors(p1 and p2) are hard(take a lot of time) to calculate if you know `n`, as the numbers get large. While multiplying large numbers(`p1 * p2`) is far easier to do.

So, a public key contains 2 numbers, a large number (`n`) and an exponent(`e`). While a private key, needs the decryption exponent (`d`), which can undo the effect of the encryption exponent (`e`).

---

With OpenSSL and Linux:
## 1. Signing a message and verifying the signature
```bash
# Generate a set of private/public keys
openssl genrsa -out ./private.key 1024 && openssl rsa -in ./private.key -pubout -out ./public.key

# Prepare a message, that will be signed
echo "this is just a test" > ./message

# Generate the signature (binary and base64)
openssl dgst -sha256 -sign ./private.key -out ./signature ./message
# To print the signature as base64 run: base64 -w0 ./signature ; echo

# Send the ./public.key, ./message and the ./signature[.b64]
mkdir sender && mv * sender ; mkdir receiver && cd receiver
cp ../sender/{public.key,message,signature} .

# Verify the if the message was actually sent by the person holding the private key.
# To perform this verification, you need the public key, the message and the signature.
openssl dgst -sha256 -verify ./public.key -signature ./signature ./message
# It will print "Verified OK"(exit code 0) or "Verification Failure"(exit code 1)
```

## 2. Encrypt and decrypt
```bash
# Generate a set of private/public keys
openssl genrsa -out ./private.key 1024 && openssl rsa -in ./private.key -pubout -out ./public.key

# Encrypt bytes with public key, and send publicly. Only user with private key can decrypt message
echo "secret message" > ./plain.text
openssl rsautl -encrypt -pubin -inkey ./public.key -in ./plain.text -out ./encrypted.bytes

# Decrypt with private key
openssl rsautl -decrypt -in ./encrypted.bytes -out ./decrypted.bytes -inkey ./private.key

# The original and decrypted files should match
diff -y ./plain.text ./decrypted.bytes
```

## 3. Formats
There are two formats:

#### 3.1. DER
which is a binary format that uses an [ASN1](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One) [DER](https://en.wikipedia.org/wiki/X.690#DER_encoding) encoded form.

#### 3.2. PEM
which consists of the DER format base64 encoded with additional header and footer lines:
```txt
The PEM private key format uses the header and footer lines:
  -----BEGIN RSA PRIVATE KEY-----
  -----END RSA PRIVATE KEY-----

The PEM public key format uses the header and footer lines:
  -----BEGIN PUBLIC KEY-----
  -----END PUBLIC KEY-----

The PEM RSAPublicKey format uses the header and footer lines:
  -----BEGIN RSA PUBLIC KEY-----
  -----END RSA PUBLIC KEY-----
```

#### 3.3. Convert RSA PEM PKCS#1 key to RSA PEM PKCS#8
```bash
# This will generate an RSA PKCS#1 private key
> openssl genrsa -out ./private.key 1024
> cat ./private.key
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----

# To convert it to PKCS#8 you can:
> openssl pkcs8 -topk8 -inform PEM -outform PEM -nocrypt -in private.key -out private.pkcs8.key
> cat private.pkcs8.key
-----BEGIN PRIVATE KEY-----
...
-----END PRIVATE KEY-----
```

#### 3.4. Key content
```bash
# Showing the numbers inside a private key
> openssl pkey -in private.key -text
-----BEGIN PRIVATE KEY-----
MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAJ/5ReH2133izTRl
QSYGjoJZqD4TkOE2aX8hxvG7wIY4WYc0hE+eSb7WZ2SzRiTeZ104GQIVfRjWvb0t
9GBrAZICZCp/5B9dp9Z3WqGpK7PCAVjbTYu+Z3qjr1Eeizo8xt7+sHmCOlH1pWep
LIyh2yvfvMhNWDEBnlfB/J57osblAgMBAAECgYBb3acu6zS2mv7ift8ZuhwuaNQ/
ybaiTj/o/PmlKf+WVFe9WAA/RJPu3msDnhvC4mETXDqoQrTTBcZyFCjJEcoVKYv1
GElfJqjp3JLBYwkxP6kQ3YffE2L2PtgY7OG92cfnJ919/mDQEy/AJGeOw3IRXc6X
qa8TJUKg0GlrU4RfYQJBAM2IBVRag/nPVLKVHWDvKOsdJBK0BnG/tFuIcUoRB0NW
eE/xiKr6fcFpO0Hafuz+j1z8wD7tWyQgm54zTOOd020CQQDHQXBaVxPSh8SvZtZk
h5UAvH3SQrUJDhhtIExWpySXTIUhd42JZJW2YeAtZSxe5aAjHJzb4QEHkG1C7pbz
A55ZAkB6mmdDeHM9s3X8yYKq7j9kcQ+xsH4foJHAAFZELoA8pPpEBfrWs3IMy+8z
S1lnmjp+567uWryBgooSBtwY827JAkAEhJlVkw/iAC4XhA9sbB6Wy69WqyiLsgQf
xVG1zUhpHdUO8zUEXoF+hy2cGeUtqas94JI18h4h28Z+dAZ8MCLRAkEAmN4Ib+Ha
vvXck11X/M7w6ri28S9afjbRwbuY0iYN8VrkekhuL1t70AR1JuA/aJF7hWyjOeXF
sqYyeKAsNg/QUg==
-----END PRIVATE KEY-----
RSA Private-Key: (1024 bit, 2 primes)
modulus:
    00:9f:f9:45:e1:f6:d7:7d:e2:cd:34:65:41:26:06:
    8e:82:59:a8:3e:13:90:e1:36:69:7f:21:c6:f1:bb:
    c0:86:38:59:87:34:84:4f:9e:49:be:d6:67:64:b3:
    46:24:de:67:5d:38:19:02:15:7d:18:d6:bd:bd:2d:
    f4:60:6b:01:92:02:64:2a:7f:e4:1f:5d:a7:d6:77:
    5a:a1:a9:2b:b3:c2:01:58:db:4d:8b:be:67:7a:a3:
    af:51:1e:8b:3a:3c:c6:de:fe:b0:79:82:3a:51:f5:
    a5:67:a9:2c:8c:a1:db:2b:df:bc:c8:4d:58:31:01:
    9e:57:c1:fc:9e:7b:a2:c6:e5
publicExponent: 65537 (0x10001)
privateExponent:
    5b:dd:a7:2e:eb:34:b6:9a:fe:e2:7e:df:19:ba:1c:
    2e:68:d4:3f:c9:b6:a2:4e:3f:e8:fc:f9:a5:29:ff:
    96:54:57:bd:58:00:3f:44:93:ee:de:6b:03:9e:1b:
    c2:e2:61:13:5c:3a:a8:42:b4:d3:05:c6:72:14:28:
    c9:11:ca:15:29:8b:f5:18:49:5f:26:a8:e9:dc:92:
    c1:63:09:31:3f:a9:10:dd:87:df:13:62:f6:3e:d8:
    18:ec:e1:bd:d9:c7:e7:27:dd:7d:fe:60:d0:13:2f:
    c0:24:67:8e:c3:72:11:5d:ce:97:a9:af:13:25:42:
    a0:d0:69:6b:53:84:5f:61
prime1:
    00:cd:88:05:54:5a:83:f9:cf:54:b2:95:1d:60:ef:
    28:eb:1d:24:12:b4:06:71:bf:b4:5b:88:71:4a:11:
    07:43:56:78:4f:f1:88:aa:fa:7d:c1:69:3b:41:da:
    7e:ec:fe:8f:5c:fc:c0:3e:ed:5b:24:20:9b:9e:33:
    4c:e3:9d:d3:6d
prime2:
    00:c7:41:70:5a:57:13:d2:87:c4:af:66:d6:64:87:
    95:00:bc:7d:d2:42:b5:09:0e:18:6d:20:4c:56:a7:
    24:97:4c:85:21:77:8d:89:64:95:b6:61:e0:2d:65:
    2c:5e:e5:a0:23:1c:9c:db:e1:01:07:90:6d:42:ee:
    96:f3:03:9e:59
exponent1:
    7a:9a:67:43:78:73:3d:b3:75:fc:c9:82:aa:ee:3f:
    64:71:0f:b1:b0:7e:1f:a0:91:c0:00:56:44:2e:80:
    3c:a4:fa:44:05:fa:d6:b3:72:0c:cb:ef:33:4b:59:
    67:9a:3a:7e:e7:ae:ee:5a:bc:81:82:8a:12:06:dc:
    18:f3:6e:c9
exponent2:
    04:84:99:55:93:0f:e2:00:2e:17:84:0f:6c:6c:1e:
    96:cb:af:56:ab:28:8b:b2:04:1f:c5:51:b5:cd:48:
    69:1d:d5:0e:f3:35:04:5e:81:7e:87:2d:9c:19:e5:
    2d:a9:ab:3d:e0:92:35:f2:1e:21:db:c6:7e:74:06:
    7c:30:22:d1
coefficient:
    00:98:de:08:6f:e1:da:be:f5:dc:93:5d:57:fc:ce:
    f0:ea:b8:b6:f1:2f:5a:7e:36:d1:c1:bb:98:d2:26:
    0d:f1:5a:e4:7a:48:6e:2f:5b:7b:d0:04:75:26:e0:
    3f:68:91:7b:85:6c:a3:39:e5:c5:b2:a6:32:78:a0:
    2c:36:0f:d0:52



# Showing the contents of a public key
> openssl pkey -in public.key -pubin -text
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCf+UXh9td94s00ZUEmBo6CWag+
E5DhNml/Icbxu8CGOFmHNIRPnkm+1mdks0Yk3mddOBkCFX0Y1r29LfRgawGSAmQq
f+QfXafWd1qhqSuzwgFY202Lvmd6o69RHos6PMbe/rB5gjpR9aVnqSyModsr37zI
TVgxAZ5Xwfyee6LG5QIDAQAB
-----END PUBLIC KEY-----
RSA Public-Key: (1024 bit)
Modulus:
    00:9f:f9:45:e1:f6:d7:7d:e2:cd:34:65:41:26:06:
    8e:82:59:a8:3e:13:90:e1:36:69:7f:21:c6:f1:bb:
    c0:86:38:59:87:34:84:4f:9e:49:be:d6:67:64:b3:
    46:24:de:67:5d:38:19:02:15:7d:18:d6:bd:bd:2d:
    f4:60:6b:01:92:02:64:2a:7f:e4:1f:5d:a7:d6:77:
    5a:a1:a9:2b:b3:c2:01:58:db:4d:8b:be:67:7a:a3:
    af:51:1e:8b:3a:3c:c6:de:fe:b0:79:82:3a:51:f5:
    a5:67:a9:2c:8c:a1:db:2b:df:bc:c8:4d:58:31:01:
    9e:57:c1:fc:9e:7b:a2:c6:e5
Exponent: 65537 (0x10001)
```

## 4. RSA PEM key examples
#### 4.1. RSA Private key
This is how you can generate a "private" key and how looks like:
```bash
paul:test> openssl genrsa -out ./private.key 1024
Generating RSA private key, 1024 bit long modulus (2 primes)
..........+++++
..+++++
e is 65537 (0x010001)
paul:test> cat ./private.key
-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQCl+21UgkX1zKRlLaohNkUTPs/NHJbdU6FspZmvK+4gX6iST0Jo
KhgE9Yhhqda+nxtjvsJwJK8WpHs7t8uPmwMrGONw5Zxj5HR9X3W/gOsA/J6wLnCB
ny5lhrbaXETgcxe5xR2QGHO9AHMrIbEFhcR8f9NFinJUyC5QZrz3Sr4iqwIDAQAB
AoGBAICWbHjQBAsM4z9PRUI9nP3v52TsBSSqKaDWGl3PFsgV066loLi6A6mz3lhr
D2bWNI3ttwzSHqLYAnCdTyKN4ME/JaxL3ZGG4c9ieTSyLzBbHlMRasdXP3UVGtvs
uaYqZOW7iYE3ey5JStgnBivY8n7LIDQiNmPmys5up2lZmfthAkEA00GM9Xt7Sk7f
VfrPzb3hI5Hg5X7mt6ivgc28WyT33Om9Tf4Xlz4FVuQYMprO+xmt9CF34Vc9//tG
XjSpwo5ATwJBAMkjF91IG/1MklZ7Hz2IjlcHnYfCd56JyI0hLkMrOo01ltA7TLGN
TPz9nFpz4beM+pDINuNcM+ZG+tGfzJv0pOUCQHh19mS8Rq82jk8+t2PAFDLuKelz
FShAveMsZ20phVSoy9M/QkBxkyXa5plkgQXZvMFqnCsYTjg7FgL90Jcp+i0CQDOy
xnaFC1Su8soxuVTqnZN3DKGRdYeVaKwFxEtVeCZFiO8a3tqgNBKu6RpCwNiZ7ul5
3MnRsDFXOy7YQRIw7pUCQQC/Qfvcfz6fWancTgZrQY30tdNGhihcw1YlXFNiS8ue
H/3Wd8izXJIgg24iYFcOT9dLJmb6ZNV7BFCrDb+rip8q
-----END RSA PRIVATE KEY-----
```
Remember that this key file, contains the **entire key**. Both the private and
public parts. You can see all the key components using command:
```bash
paul:test> openssl rsa -in ./private.key -text
```
So, the following commands will only extract re-use the same information that is
already in this file.

#### 4.2. RSA public key
The RSA was the first format that was invented and it consists in two integers,
the modulus(n) and the public exponent(e). This key is encoded using a
`PKCS#1 RSAPublicKey` structure. And this is how you can generate an RSA
public key based on a private key, and how the RSA public key looks like:
```bash
paul:test> openssl rsa -in ./private.key -RSAPublicKey_out -out ./public.key.rsa
paul:test> cat ./public.key.rsa
-----BEGIN RSA PUBLIC KEY-----
MIGJAoGBAKX7bVSCRfXMpGUtqiE2RRM+z80clt1ToWylma8r7iBfqJJPQmgqGAT1
iGGp1r6fG2O+wnAkrxakezu3y4+bAysY43DlnGPkdH1fdb+A6wD8nrAucIGfLmWG
ttpcROBzF7nFHZAYc70AcyshsQWFxHx/00WKclTILlBmvPdKviKrAgMBAAE=
-----END RSA PUBLIC KEY-----
```
You can see an RSA key's contents using:
```bash
paul:test> openssl rsa -RSAPublicKey_in -in ./public.key.rsa -noout -text
```

#### 4.3. Public key
This format contains an extra `AlgorithmIdentifier` structure. It is encoded using a
`SubjectPublicKeyInfo` structure and this is how you can generate such a public key
based on a private key, and how the public key looks like:
```bash
paul:test> openssl rsa -in ./private.key -pubout -out ./public.key
paul:test> cat ./public.key
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCl+21UgkX1zKRlLaohNkUTPs/N
HJbdU6FspZmvK+4gX6iST0JoKhgE9Yhhqda+nxtjvsJwJK8WpHs7t8uPmwMrGONw
5Zxj5HR9X3W/gOsA/J6wLnCBny5lhrbaXETgcxe5xR2QGHO9AHMrIbEFhcR8f9NF
inJUyC5QZrz3Sr4iqwIDAQAB
-----END PUBLIC KEY-----
```
You can see a public key's contents using:
```bash
paul:test> openssl rsa -pubin -in ./public.key -noout -text
```
**Note**! that given one private key, it's public key and RSA public key will
NOT be the same. The public key in DER format(before encoding to base64) has an
extra information, the `AlgorithmIdentifier` structure, which is constant for
`RSA PKCS#1`. For more info see:
[this link](https://stackoverflow.com/a/29707204).


#### 4.4. SSH public key
When you use the command `ssh-keygen` to generate a pair of private/public keys,
the private key will look like the one above, but the **public** key will look
like:
```bash
# For example:
paul:test> ssh-keygen -f ./private.key -P "" -m PEM -C "key@generic" && mv private.key.pub public.key.ssh
paul:test> cat public.key.ssh
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6+aiD/ynCfXp5hCzP2hKibYtYbNVszA32XRTo71Xc0yarvKQkpmqMbCZuKnVcIvRuM3lX7pjn5YexjKf874psQyrZToQcq4krKgmON4venkv6b6aZ2dC1IWk7zwDBd/Q4mnkVLCqUUPSw/oJ8NaPAKw3F+6WtOQEQT/Hw1Ut52sE6rQ+AaYLrCIb94Bzzxj1qxBbjz7/rqQS0kQduvfNpN/eyYtDxdPWqJOktca9lLKqXqJL6PRcYdpAok/H+FBHG2JZswrNnHKDBgWRu2GWhZDe8A1ekOuakUnFqnCiejWqtsHESwvNHZVhJ0YmvGUy3hAOfWbOe9pPpXSnizzLP key@generic
```

#### 4.5. Conversions
To convert between the types you can use:
```bash
# "SSH public key" -> classic "public key" format:
ssh-keygen -f ./public.key.ssh -e -m pkcs8 > ./public.key

# "SSH public key" -> "RSA public key" format:
ssh-keygen -f ./public.key.ssh -e -m pem > ./public.key.rsa

# "public key" -> "SSH public key" format:
ssh-keygen -f ./public.key -i -m pkcs8 > ./public.key.ssh

# "public key" -> "RSA public key" format:
openssl rsa -in ./public.key -out ./public.key.rsa -pubin -RSAPublicKey_out 

# "RSA public key" -> "SSH public key" format:
ssh-keygen -f ./public.key.rsa -i -m pem > ./public.key.ssh

# "RSA public key" -> classic "public key" format:
openssl rsa -RSAPublicKey_in -in ./public.key.rsa -pubout -out ./public.key


# ====
# PEM classic "public key" -> DER classic "public key"
openssl rsa -pubin -inform pem -outform der -in ./public.key -out ./public.key.der
```


<table border="1px">
<tr>
  <td>L2C</td>
  <td>STD</td>
  <td>RSA</td>
  <td>SSH</td>
</tr>
<tr>
  <td>STD</td>
  <td>-</td>
  <td><pre>openssl rsa -in ./public.key -out ./public.key.rsa -pubin -RSAPublicKey_out</pre></td>
  <td><pre>ssh-keygen -f ./public.key -i -m pkcs8 > ./public.key.ssh</pre></td>
</tr>
<tr>
  <td>RSA</td>
  <td><pre>openssl rsa -RSAPublicKey_in -in ./public.key.rsa -pubout -out ./public.key</pre></td>
  <td>-</td>
  <td><pre>ssh-keygen -f ./public.key.rsa -i -m pem > ./public.key.ssh</pre></td>
</tr>
<tr>
  <td>SSH</td>
  <td><pre>ssh-keygen -f ./public.key.ssh -e -m pkcs8 > ./public.key</pre></td>
  <td><pre>ssh-keygen -f ./public.key.ssh -e -m pem > ./public.key.rsa</pre></td>
  <td>-</td>
</tr>
</table>
---
---
---

## 5. Certificates
#### 5.1. Generate a self-signed certificate
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/C=US/ST=Oregon/L=Portland/O=Company Name/OU=Org/CN=www.example.com"
```
Will generate a private `key.pem` file(also contains the public part), and a certificate file: `cert.pem`(public file)

#### 5.2. Generate a Certificate Authority (CA) and with it sign new certificates
Create this `ca.cnf` file:
```bash
# we use 'ca' as the default section because we're using the ca command
[ ca ]
default_ca = my_ca

[ my_ca ]
#  a text file containing the next serial number to use in hex. Mandatory.
#  This file must be present and contain a valid serial number.
serial = ./serial

# the text database file to use. Mandatory. This file must be present though
# initially it will be empty.
database = ./index

# specifies the directory where new certificates will be placed. Mandatory.
new_certs_dir = ./newcerts

# the file containing the CA certificate. Mandatory
certificate = ./ca.crt

# the file contaning the CA private key. Mandatory
private_key = ./ca.key

# the message digest algorithm. Remember to not use MD5
default_md = sha1

# for how many days will the signed certificate be valid
default_days = 365

# a section with a set of variables corresponding to DN fields
policy = my_policy

[ my_policy ]
# if the value is "match" then the field value must match the same field in the
# CA certificate. If the value is "supplied" then it must be present.
# Optional means it may be present. Any fields not mentioned are silently
# deleted.
countryName = match
stateOrProvinceName = supplied
organizationName = supplied
commonName = supplied
organizationalUnitName = optional
commonName = supplied
```

Then continue with:

```bash
# Generate CA key and certificate
openssl genrsa -out ca.key 1024 &&
openssl req -x509 -new -nodes -sha256 -days $((5 * 365)) \
  -key ca.key \
  -out ca.crt \
  -subj "\
/C=US\
/ST=Oregon\
/L=Portland\
/O=Grozav Certfication Authority\
/OU=Org\
/CN=ca.grozav.info\
" &&

# Init CA database
mkdir newcerts &&
touch index &&
echo 01 > serial &&


out_file="my.crt" &&
details="\
/C=US\
/ST=Oregon\
/L=Portland\
/O=Company 1\
/OU=Org\
/CN=www.one.com\
" &&

# Generate Certificate Signing Request (CSR)
openssl req -new \
  -key ca.key \
  -out my.csr \
  -subj "${details}" &&

# Show .csr info:
openssl req -text -noout -verify -in my.csr &&

# Generate certificate signed by CA
ca_output="$(openssl ca -batch -verbose -out /dev/null \
  -config ca.cnf \
  -infiles my.csr \
  2>&1 )" &&
cert_file="$(echo "${ca_output}" |
  grep "^writing ./newcerts/" | awk '{print $2}'
  )" &&

# Verify that the certificate is signed by our CA
openssl verify -verbose -CAfile ca.crt ${cert_file} &&

# Save just the certificate as Base64, not the decoded prefix
cat ${cert_file} | grep -A999 "^-----BEGIN CERTIFICATE-----$" | cat - \
  > ${out_file} &&

# This is your certificate:
cat ${out_file}
```
Will generate a private `key.pem` file(also contains the public part), and a certificate file: `cert.pem`(public file)

#### 5.3. Other operations
```bash
# Check that certificate is offered by web server, using a debugger
# client which shows certificate info:
openssl s_client -connect 127.0.0.1:443

# Get the certificate from a server
echo -n | openssl s_client -connect google.com:443 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > downloaded.cert

# Get info from a certificate file
openssl x509 -startdate -enddate -issuer -subject -email -serial -fingerprint -noout -in cert.pem

# Get all info from a certificate file
openssl x509 -text -noout -in cert.pem

# Check if a public key is the pair of a private key by comparing their
# modulus. If the private and public key files are a pair, then, their
# modulus should be the same:
openssl rsa -modulus -noout -in private.key
openssl rsa -modulus -pubin -noout -in public.key
# This output should be 0 :
diff <(openssl rsa -modulus -noout -in private.key) <(openssl rsa -modulus -pubin -noout -in public.key) | wc -l

# Check that a certificate is signed using a CA certificate:
openssl verify -verbose -CAfile ca.crt my.crt
# Should say: "my.crt: OK"

```


## See also:
1. [KhanAcademy - Cryptography](https://www.khanacademy.org/computing/computer-science/cryptography) - especially RSA encryption explained - parts [1](https://youtu.be/EPXilYOa71c), [2](https://youtu.be/IY8BXNFgnyI), [3](https://youtu.be/cJvoi0LuutQ) and [4](https://youtu.be/UjIPMJd6Xks)
2. Different types of public keys: [https://stackoverflow.com/a/29707204](https://stackoverflow.com/a/29707204)
3. Reading keys in C: [https://www.openssl.org/docs/man1.0.2/man3/PEM_read_bio_RSA_PUBKEY.html](https://www.openssl.org/docs/man1.0.2/man3/PEM_read_bio_RSA_PUBKEY.html)
