---
layout: post
title: "DNS delegation and cert manager"
date: 2026-06-08
categories: [systems]
---

So far I have been exposing my services on
`service-name.k8s.server.paul.grozav.info`, and used cert-manager to generate
a certificate for each sub-domain, for each `service-name`. Certificates are
issued by Let's Encrypt, so before issuing a certificate for a specific domain,
LE (Let's Encrypt) needs to verify you are the owner of that domain. And the
proof of ownership is done using a challenge. For specific domain certificate
requests, an http challenge is enough. LE can make an HTTP request and expect
a token in response, to prove you own that domain.

But if you want to generate a wildcard certificate, that works for any direct
(1 level below) sub-domain, you can't use an HTTP challenge, as that only proves
you have admin access to a specific domain. For a wildcard certificate you need
to prove you have admin access to the NS (Name Server). So for this you use a
DNS challenge, which requires you to add a TXT record on your domain, proving
you own it. In my case, a TXT record on `k8s.server.paul.grozav.info` .
