## Steps to recreate

A private repository is needed.
This can be a dummy. `argo-repo.yaml` needs to be updated before-hand.

```
# create a dummy cluster
kind create cluster

# deploy argocd on top of it
helm upgrade -i -n argocd argocd argo/argo-cd --version 5.43.0 -f values.yaml

# deploy projects
kubectl apply -f argo-projects.yaml

# deploy repo (make sure to have updated the file with correct url and key)
kubectl apply -f argo-repo.yaml

# get admin account
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

# on a different terminal port forward
kubectl port-forward service/argocd-server -n argocd 8080:443

# login as admin user to set alice password
argocd login localhost:8080
argocd account update-password --account alice

# Then login to the ui using alice user and pass
# create an app on proj-b using repo-A.
# it can connect and deploy just fine
```
