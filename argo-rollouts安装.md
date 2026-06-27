kubectl create ns argo-rollouts
kubectl apply -f install.yaml -n argo-rollouts
kubectl apply -f dashboard-install.yaml -n argo-rollouts
kubectl apply -f argo-rollouts-dashboard-Ingress.yml -n argo-rollouts