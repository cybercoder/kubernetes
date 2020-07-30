### Add Helm Repo:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```
### Install using Helm 3:
```
kubectl create namespace kubeapps
helm install kubeapps --namespace kubeapps bitnami/kubeapps --set useHelm3=true
```

### create operator service account and obtain the token:
```
kubectl create serviceaccount kubeapps-operator
kubectl create clusterrolebinding kubeapps-operator --clusterrole=cluster-admin --serviceaccount=default:kubeapps-operator
kubectl get secret $(kubectl get serviceaccount kubeapps-operator -o jsonpath='{.secrets[].name}') -o jsonpath='{.data.token}' | base64 --decode
```

### run kubeapps using ```port-forward```:
```
  kubectl port-forward --namespace kubeapps --address 192.168.88.48 service/kubeapps 8080:80
```
#### Browse : ```192.168.88.48:8080```
