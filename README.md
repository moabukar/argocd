# ArgoCD Project (Local Env)

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

1. kubectl -n argocd patch secret argocd-secret  -p '{"data": {"admin.password": null, "admin.passwordMtime": null}}'

2. kubectl -n argocd scale deployment argocd-server --replicas=0

once scaled-down, make sure to scale back up and wait a few minutes before

3. kubectl -n argocd scale deployment argocd-server --replicas=1

4. New password of the ArgoCD will be your api-server pod name with the numbers at the end name (kubectl -n argocd get po >> to find pod name)

i.e login:

user: admin
pass: argocd-server-6cdb9b4b84-jvl58

```


## Step 3 - App deployment (ArgoCD in Action)

```sh

kubectl -n argocd apply -f ./argocd/app.yaml

```

## Step 4 - Wait a moment and check CD is working

```sh

kubectl -n example-app get po

```

## Step 5 - Test ArgoCD capabilities

1. make a change in the replicas of the deployment in (./app/deployments/deployment.yaml)
2. Make a PR and push to master
3. Wait for ArgoCD to sync with cluster or sync manually. 
4. Watch the pods terminate/increase.

## Misc - ArgoCD CLI

1. login to ArgoCD CLI

```sh

argocd_server=`kubectl -n argocd get pods -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2`

argocd login 127.0.0.1:8080 \
  --username=admin \
  --password="${argocd_server}" \
  --insecure
```

2. Use the ArgoCD CLI

```sh

argocd app get example-app ## list ArgoCD app on the CLI ("example-app" - name of App)

argocd app sync example-app ## sync the "example-app" with the repo
```