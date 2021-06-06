# argocd-fun

Setup cluster of your choosing; using kind here.
```
kind create cluster --name argocd-cluster
```
then install argo-cd chart
```
helm install argo-cd charts/argo-cd/
```
Wait for that to start, then access argo-cd UI w/
```
kubectl port-forward svc/argo-cd-argocd-server 8080:443
```
then open `http://localhost:8080` in browser and login.
UI login: username 	`admin` and get password with
```
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
then allow argo-cd to manage itself and other apps
```
helm template apps/ | kubectl apply -f -
```
Argo-CD is now setup to manage itself so we can delete it from helm via
```
kubectl delete secret -l owner=helm,name=argo-cd
```
From this point on, to add new apps we no longer need to helm install or kubectl apply ever again. You add apps by commiting a new application manifest to `apps/templates`. The new applications can link to public helm repositories, public or private git reps (needs creds) and the root application will make sure they are synced and deployed/removed from cluster.

This repo is largely adapted from this blog [post](https://www.arthurkoziel.com/setting-up-argocd-with-helm/)

