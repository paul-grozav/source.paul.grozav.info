---
layout: page
ptitle: Kubernetes k8s
---

### Kubernetes (K8s)
```bash
# Start pod(container) interactively and delete it at the end 
kubectl -n my-ns run my-test-pod --image=alpine:3.15.1 --env k1=v1 --env k2=v2 --stdin --tty --rm=true -- /bin/sh
```