---
layout: page
ptitle: Cryptography
---
With OpenSSL and Linux:
## 1. Signign a message and verifying the signature
```bash
# Create a playground
mkdir test && cd test

# Generate a set of private/public keys
#ssh-keygen -f ./key -P "" -m PEM -C "key@generic" && cat ./key ./key.pub
openssl genrsa -out ./private.key 4096 && openssl rsa -in ./private.key -pubout -out ./public.key


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
