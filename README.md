# ArgoCD Local Env

## Pre-requisites 

- Ensure you have a container runtime enginer installed (docker-dekstop or rancher-desktop)
- Ensure you have kubectl
## Step 1

```sh

1. kubectl create ns argocd

2. kubectl -n argocd apply -f ./argocd/install.yaml

3. kubectl -n argocd port-forward svc/argocd-server 8080:443

```

## Step 2

```sh

1. kubectl -n patch secret argocd-secret  -p '{"data": {"admin.password": null, "admin.passwordMtime": null}}'

2. kubectl -n argocd scale deployment argocd-server --replicas=0

once scaled-down, make sure to scale back up and wait a few minutes before

3. kubectl -n argocd scale deployment argocd-server --replicas=1

4. New password of the ArgoCD will be your api-server pod name with the numbers at the end name (kubectl -n argocd get po >> to find pod name)

i.e login:

user: admin
pass: argocd-server-6cdb9b4b84-jvl58

```


## Step 3

