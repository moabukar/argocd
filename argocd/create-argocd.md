
CLI
```
argocd app create --name hello \
--repo https://github.com/moabukar/argocd \
--dest-server https://kubernetes.default.svc \
--dest-namespace mo --path kubernetes
```

YAML

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: hello
  namespace: mo
spec:
  project: default
  source:
    repoURL: https://github.com/moabukar/argocd.git
    targetRevision: HEAD
    path: app
  destination:
    server: https://kubernetes.default.svc
    namespace: mo
```

