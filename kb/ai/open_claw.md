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

# Ollama
My claw connects to my local ollama, that has multiple models available: `ollama list`.

I can pull any model using `ollama pull nimble-v3` to pull this model: https://ollama.com/library/nimble-v3 .

You can easily see available models on their site: https://ollama.com/search?c=local .

Once you are done with a model, remove it with `ollama rm nimble-v3` .

To test it fast with a curl request you can run: `time curl http://localhost:11434/api/generate -d '{"model": "llama3.1:8b", "prompt": "Why is the sky blue?", "stream": false}'` .

See also:
- https://openwebui.com/
- https://github.com/SYSTRAN/faster-whisper
- ghcr.io/berriai/litellm:main-latest