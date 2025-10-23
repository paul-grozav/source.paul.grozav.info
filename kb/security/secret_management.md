---
layout: page
title: Secret management
---

---

[HashiCorp Vault](https://www.vaultproject.io/) or it's Open Source alternative
[OpenBao](https://openbao.org/), just like
[AWS Secrets Manager](https://aws.amazon.com/secrets-manager/) are designed to
store secrets like passwords and tokens. And it is a very interesting adventure
going down the road to understand how to use these secrets in an application,
especially if you don't want to protect another secret required to authenticate
into OpenBao itself, to get the password you need. The best solution I found to
this problem, so far, is to use it in conjunction with an infrasturcture like
Kubernetes. K8s being the infrastructure that starts and runs your application,
it is capable of injecting a JWT into the application. Then the application
authenticates into OpenBao with that token. In turn, OpenBao verifies the
validity of the token with K8s itself, who emitted it.
<br/><br/>
However, if the secret you are trying to store is an encryption key, then you
shouldn't use the same set of technologies. In this case, what you would like to
use is a Key Management Service (KMS). However, a KMS software will, usually,
not give you the key to decrypt your data. Instead, you should be sending the
data to the KMS service, and it will decrypt it and return the decoded data. So,
for example your data could be encrypted with a data-key, that is stored
encrypted next to the data. When you want to read your data, you send the
data-key (a small file) to the KMS to be decrypted, and then use the decrypted
data-key to decrypt your actual data.
