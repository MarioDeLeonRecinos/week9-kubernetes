# week9-kubernetes-helm

## Gcloud connect cluster

gcloud container clusters get-credentials my-gke-cluster

gcloud container clusters get-credentials my-gke-cluster --zone us-west1-c --project week9-356019  && kubectl get secret gcr-json-key --namespace my-website -o yaml

## Helm install nginx-controller and cert-manager

helm install my-nginx-ingress ingress-nginx/ingress-nginx -n nginx-cert-manager -f .\values\nginx-controller.yaml

helm install my-cert-manager cert-manager/cert-manager --version 1.8.2 -n nginx-cert-manager

## Install cluster-issuer

kubectl apply -f cluster-issuer.yaml -n nginx-cert-manager

## Install argo-cd and recover password

helm install my-argo-cd argo/argo-cd --version 4.9.5 -n argocd -f .\argo-cd-values.yaml

helm install my-argo-rollouts argo/argo-rollouts --version 2.17.0

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

PQ5CLqhj4KSOKTns

## After install of blackbox-exporter and prometheus-stack in argo-cd

kubectl apply -f .\probe.yaml

## Helm install legacy commands

helm install my-kube-prometheus-stack prometheus-community/kube-prometheus-stack --version 36.2.1 -n kube-grafana -f .\values-kube-prometheus-stack.yaml

helm install my-prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter --version 5.8.2 -n kube-grafana

helm install my-skooner christianknell/skooner --version 0.0.3 -n dashboard-skooner -f .\values-skooner.yaml

helm install my-deploy-website my-deploy-week4 -n my-website

## Recover secrets

kubectl get secrets -n dashboard-skooner
kubectl -n dashboard-skooner describe secret my-skooner-token-cxwx5

## Secret create in Linux base console

kubectl create secret docker-registry gcr-json-key \
 --docker-server=us.gcr.io \
 --docker-username=_json_key \
 --docker-password="$(cat ~/json-key-file.json)" \
 --docker-email=cloud-build@week9-356019.iam.gserviceaccount.com -n my-website

## Recovered names of pods for High Avaliability Vault not working

my-vault-0.my-vault-internal:8201

my-vault-1.my-vault-internal:8201

my-vault-2.my-vault-internal:8201

kubectl port-forward my-vault-1 8200:8200

kubectl exec my-vault-0 -n vault -- vault operator init -key-shares=1 -key-threshold=1 -format=json > cluster-keys.json

## Recover secret

kubectl get secret example-sync -o jsonpath='{.data.*}' -n wordpress | base64 -d 