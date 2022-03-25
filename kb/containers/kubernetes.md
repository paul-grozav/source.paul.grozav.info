---
layout: page
ptitle: Kubernetes k8s
---

### Kubernetes (K8s)
```bash
# Start pod(container) interactively and delete it at the end 
kubectl -n my-ns run pgrozav-test --image=alpine:3.15.1 --stdin --tty --rm=true -- /bin/sh
```