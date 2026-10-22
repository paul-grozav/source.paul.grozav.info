---
layout: page
title: Open Claw
---

Simply shell into the openclaw container: `kubectl -n ai exec -it statefulset/my-agent -c openclaw -- bash -c "cd ~/.openclaw/workspace && export GIT_CONFIG_PARAMETERS=\"'safe.directory=*'\" && bash"`

I like to init it's home folder using: `kubectl --kubeconfig -n ai exec -it statefulset/my-agent -c openclaw -- bash -c "cd ~/.openclaw/workspace && export GIT_CONFIG_PARAMETERS=\"'safe.directory=*'\" && git remote add origin https://user:PAT@github.com/paul-grozav/openclaw.git && git fetch origin -p && git checkout -f devel"`
To give it access to a gitlab project:
```yaml
settings:
  access_tokens:
  - token_name: openclaw
    scopes:
    - api
    - read_api
    - create_runner
    - manage_runner
    - k8s_proxy
    - read_repository
    - write_repository
    - ai_features
    expires: 1 year from creation time
    role: Developer
```