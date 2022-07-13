# week9-kubernetes-helm

gcloud container clusters get-credentials my-gke-cluster

gcloud container clusters get-credentials my-gke-cluster --zone us-west1-c --project week9-356019  && kubectl get secret gcr-json-key --namespace my-website -o yaml

helm install my-nginx-ingress nginx/nginx-ingress --version 0.13.2 -n nginx-cert-manager -f .\nginx-controller.yaml

helm install my-cert-manager cert-manager/cert-manager --version 1.8.2 -n nginx-cert-manager

kubectl apply -f cluster-issuer.yaml -n nginx-cert-manager

helm install my-argo-cd argo/argo-cd --version 4.9.5 -n argocd -f .\argo-cd-values.yaml

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

kubectl apply -f .\probe.yaml

helm install my-kube-prometheus-stack prometheus-community/kube-prometheus-stack --version 36.2.1 -n kube-grafana -f .\values-kube-prometheus-stack.yaml

helm install my-prometheus-blackbox-exporter prometheus-community/prometheus-blackbox-exporter --version 5.8.2 -n kube-grafana

helm install my-skooner christianknell/skooner --version 0.0.3 -n dashboard-skooner -f .\values-skooner.yaml

kubectl get secrets -n dashboard-skooner
kubectl -n dashboard-skooner describe secret my-skooner-token-tfxp8

helm install my-deploy-website my-deploy-week4 -n my-website

#Secret create in Linux base console
kubectl create secret docker-registry gcr-json-key \
 --docker-server=us.gcr.io \
 --docker-username=_json_key \
 --docker-password="$(cat ~/json-key-file.json)" \
 --docker-email=cloud-build@week9-356019.iam.gserviceaccount.com -n my-website
