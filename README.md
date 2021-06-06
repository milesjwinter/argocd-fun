# argocd-fun

Setup cluster
```
kind create cluster --name argocd-cluster
```
then install argo-cd chart
```
helm install argo-cd charts/argo-cd/
```
Wait for that to start, then allow argo-cd to manage itself and other apps
```
helm template apps/ | kubectl apply -f -
```
Argo-CD is now setup to manage itself so we can delete it from helm via
```
kubectl delete secret -l owner=helm,name=argo-cd
```
Then access argo-cd UI w/
```
kubectl port-forward svc/argo-cd-argocd-server 8080:443
```
then open `http://localhost:8080` in browser and login.
UI login: username 	`admin` and get password with
```
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
