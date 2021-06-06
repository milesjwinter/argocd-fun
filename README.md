# argocd-fun

UI login: username 	`admin` and get password with
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
