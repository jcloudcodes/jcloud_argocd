# Install Argo CD on GKE

## 1) Create namespace
kubectl create ns argocd

## 2) Install Argo CD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## 3) Wait for pods
kubectl get pods -n argocd

## 4) Access UI (port-forward)
kubectl port-forward svc/argocd-server -n argocd 8080:443

Open: https://localhost:8080

## 5) Get initial admin password
kubectl get secret argocd-initial-admin-secret -n argocd \
  -o jsonpath="{.data.password}" | base64 -d; echo

## 6) Deploy your apps
kubectl apply -f repo/environments/dev/application.yaml
kubectl apply -f repo/environments/prod/application.yaml

#Quick test commands
helm template django-app repo/charts/django-app -f repo/environments/dev/values.yaml
kubectl apply -n argocd -f repo/environments/dev/application.yaml
kubectl apply -n argocd -f repo/environments/prod/application.yaml
