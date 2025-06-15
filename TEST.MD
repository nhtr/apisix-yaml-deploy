minikube delete
minikube start
minikube status

helm repo add apisix https://charts.apiseven.com
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

minikube addons enable ingress

helm install apisix apisix/apisix --create-namespace -n apisix -f apisix-values.yaml
helm install keycloak bitnami/keycloak -n keycloak --create-namespace -f keycloak-values.yaml

kubectl port-forward svc/apisix-dashboard -n apisix 9000:80
kubectl port-forward svc/apisix-gateway 9001:80 -n apisix

sudo echo "127.0.0.1 keycloak.local" | sudo tee -a /etc/hosts
cat /etc/hosts | grep keycloak.local

minikube addons enable ingress
sudo minikube tunnel

[//]: # (kubectl port-forward svc/keycloak 8080:80 -n keycloak)

For Windows
kubectl apply -f apisix-manifest.yaml
kubectl apply -f keycloak-manifest.yaml

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.6/deploy/static/provider/cloud/deploy.yaml

Tạo file keycloak-ingress.yaml
`apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
name: keycloak-ingress
annotations:
nginx.ingress.kubernetes.io/rewrite-target: /
spec:
rules:
- host: keycloak.local
  http:
  paths:
    - path: /
      pathType: Prefix
      backend:
      service:
      name: keycloak
      port:
      number: 80`

kubectl apply -f keycloak-ingress.yaml

Ánh xạ keycloak.local về localhost
Mở file:
C:\Windows\System32\drivers\etc\hosts
Thêm dòng:
127.0.0.1 keycloak.local



