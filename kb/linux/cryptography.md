---
layout: page
ptitle: Cryptography
---
With OpenSSL and Linux:
## 1. Signing a message and verifying the signature
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

## 2. Encrypt and decrypt
```bash
# Generate private & public keys
ssh-keygen -f ./key -P "" -m PEM -C "key@test_key"

# Convert public key to pkcs8 format
ssh-keygen -f ./key.pub -e -m pkcs8 > ./pkcs8_key.pub

# Encrypt bytes with public key, and send publicly. Only user with private key can decrypt message
echo "secret message" > ./plain.text
openssl rsautl -encrypt -pubin -inkey ./pkcs8_key.pub -in ./plain.text -out ./encrypted.bytes

# Decrypt with private key
openssl rsautl -decrypt -in ./encrypted.bytes -out ./decrypted.bytes -inkey ./key

# The original and decrypted files should match
diff -y ./plain.text ./decrypted.bytes
```
