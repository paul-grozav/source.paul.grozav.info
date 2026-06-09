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

The problem is that this process needs to be completely automated. cert-manager
needs to inject a new TXT record each time the renewal process is triggered.
This is usually handled through an API provided by the NS, but some companies
do not offer an API, or you might have an on-premise installation of bind that
does not expose an API. You can surely call a custom script that can inject the
TXT record in your local bind NS, but if your NS is offered by a company, it
might block your script-based logins through mechanisms like captcha.

So, a simpler solution to this problem is to delegate the k8s subdomain to
another NS that offers an API. Not lots of platforms allow you to add a
subdomain only to their NS, and not the top-level domain. Luckily Digital Ocean:
- Allows you to add just a subdomain to their NS
- Allows you to manage the domain through an API
- Cert-manager already has support for integrating with the Digital Ocean API
- The entire thing is free, as in no financial cost

# Digital Ocean account
In your control panel, go to `Networking` (on the left menu) then select
`Domains`.

Click `Add a domain`, then enter your K8s (Kubernetes) subdomain, in my case
`k8s.server.paul.grozav.info` and click `Add Domain`.

DigitalOcean will accept it and display their free public Anycast nameservers:
- `ns1.digitalocean.com`
- `ns2.digitalocean.com`
- `ns3.digitalocean.com`

# Your existing NS company
Into your existing NS (say internet.bs), add the following records, on your top
level domain. In my case the top level domain in `grozav.info` and the records I
am adding are of type `NS` for the subdomain `k8s.server.paul` and the value is
`ns1.digitalocean.com`. So, for each NS of Digital Ocean, we create an NS record
for our subdomain, so a total of 3 NS records.

```txt
NS     k8s.server.paul     ns1.digitalocean.com
NS     k8s.server.paul     ns2.digitalocean.com
NS     k8s.server.paul     ns3.digitalocean.com
```

Once the NS records are added, add another record of type `CNAME`:
`CNAME   _acme-challenge.k8s.server.paul
_acme-challenge.k8s.server.paul.grozav.info`

"Wait, this is pointing the exact same name to itself. Isn't that a useless
loop?"

Under normal circumstances, you would be 100% correct. If everything were hosted
in one place, that record would be completely redundant. But because of your
Subdomain Delegation (sending k8s subdomain over to DigitalOcean), that specific
CNAME acts as a vital traffic signpost that breaks Let's Encrypt out of
Internet.bs and forces it onto DigitalOcean.

Without the CNAME:
1. Let's Encrypt knocks on Internet.bs's door and asks: "Give me the TXT token
at _acme-challenge.k8s.server.paul.grozav.info."
2. Internet.bs looks at its zone file. It sees your NS records delegating
traffic to DigitalOcean, but NS records only apply to things below or inside
that zone.
3. Because Internet.bs doesn't inherently know how to handle the specific
_acme-challenge prefix for a delegated zone, it will return an empty answer
(NXDOMAIN or No Record).
4. Validation fails instantly. Let's Encrypt never even thinks to look at
DigitalOcean.

With the CNAME:
1. The Internet.bs Hand-off
The resolver asks the Internet.bs nameservers: "Give me the TXT record for
_acme-challenge.k8s.server.paul.grozav.info."

Internet.bs looks at its records and sees the CNAME pointing to the exact same
name. It sends back an answer that essentially says: "I don't have a TXT record
here. But I have a CNAME rule that says this address redirects to
_acme-challenge.k8s.server.paul.grozav.info."

2. The Resolver's Memory Cache
The resolver receives this CNAME response. Because it has been told to look up
that name again, it clears its current lookup path and starts a fresh internal
query loop.

Crucially, the resolver also just learned from Internet.bs (via the NS records
you set up earlier) that the zone k8s.server.paul.grozav.info has its own
dedicated authoritative nameservers: ns1.digitalocean.com, ns2.digitalocean.com,
and ns3.digitalocean.com. The resolver stores this information in its short-term
memory (cache).

3. Going Straight to DigitalOcean
Because the resolver now knows that DigitalOcean is the absolute boss for any
traffic matching k8s.server.paul.grozav.info, it bypasses Internet.bs completely
for the second loop.

It connects directly over the internet to DigitalOcean's nameservers on Port 53
and asks: "Give me the TXT record for
_acme-challenge.k8s.server.paul.grozav.info."

DigitalOcean checks the zone, finds the temporary token your cert-manager pod
just created via the API, and hands it over. Let's Encrypt reads it, validates
your domain, and issues your wildcard certificate.

