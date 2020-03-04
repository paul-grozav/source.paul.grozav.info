---
layout: page
ptitle: Cryptography
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
#openssl base64 -in ./signature -out ./signature.b64 && awk 'BEGIN{printf "SIGNATURE="}{printf $0}END{print}' ./signature.b64

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

## 4. RSA Examples
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

#### 4.2. Public key
This is how you can generate a public key based on a private key, and how the
public key looks like:
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

#### 4.2. RSA public key
This is how you can generate an RSA public key based on a private key, and how
the RSA public key looks like:
```bash
paul:test> openssl rsa -in ./private.key -RSAPublicKey_out -out ./public.key.rsa
paul:test> cat ./public.key.rsa
-----BEGIN RSA PUBLIC KEY-----
MIGJAoGBAKX7bVSCRfXMpGUtqiE2RRM+z80clt1ToWylma8r7iBfqJJPQmgqGAT1
iGGp1r6fG2O+wnAkrxakezu3y4+bAysY43DlnGPkdH1fdb+A6wD8nrAucIGfLmWG
ttpcROBzF7nFHZAYc70AcyshsQWFxHx/00WKclTILlBmvPdKviKrAgMBAAE=
-----END RSA PUBLIC KEY-----
```
**Note**! that given one private key, it's public key and RSA public key will
NOT be the same. For some private keys, the public key will be some_32_bytes
plus the RSA plublic key.

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

# "RSA public key" -> "SSH public key" format:
ssh-keygen -f ./public.key.rsa -i -m pem > ./public.key.ssh

# "public key" -> "RSA public key" format:
openssl rsa -in ./public.key -out ./public.key.rsa -pubin -RSAPublicKey_out 
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
  <td>-</td>
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

## See also:
1. [KhanAcademy - Cryptography](https://www.khanacademy.org/computing/computer-science/cryptography) - especially RSA encryption explained - parts [1](https://youtu.be/EPXilYOa71c), [2](https://youtu.be/IY8BXNFgnyI), [3](https://youtu.be/cJvoi0LuutQ) and [4](https://youtu.be/UjIPMJd6Xks)
2. Different types of public keys: [https://stackoverflow.com/a/29707204](https://stackoverflow.com/a/29707204)
