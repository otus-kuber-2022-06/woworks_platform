# woworks_platform
woworks Platform repository

## kubernetes-intro
- Был создан Dockerfile для nginx, который показывает статические файлы из папки /app
- Был создан k8s манифест для образа созданного в предыдущем пункте. initContainers секция содержит скрипт который генерирует index.html в папке /app
- Был создан frontend-pod-healthy.yaml манифест, который запускает frontend образ. В манифесте установлено значение для переменной окружения PRODUCT_CATALOG_SERVICE_ADDR

## kubernetes-controller
- создал rs и deployment для frontend-а
- создал deployment-ы (включая blue-green и reverse) для paymentservice
- создал daemonset для node-exporter-а, изменил манифест так, чтоб pod запускался и на control-plane

## kubernetes-security
- создал несколько ServiceAccount-ов, Role-ей, RoleBinding и ClusterRoleBinding-ов

## kubernetes-networks
- создал deployment для web
- включил ipvs режим minikube
- добавил ClusterIP и LoadBalancer сервисы
- создал metallb в minikube кластере
- создал ingress

## kubernetes-volumes
- создал statefulset c Minio
- создал headless service для доступа
- создал secret и настроил использование данных из secret-а в statefulSet-е

## kubernetes-templating
- зарегистрировался на Google Cloud Platform и создал там кластер
- установил helm 3
- установил nginx-ingress чарт
- установил cert-manager чарт
- установил chartmuseum
- установил harbor
- cоздал свой helm chartchart - hipster-shop и frontend
- c помощью kubecfg создал манифесты services.jsonnet



## kubernetes-monitoring
- собрал образ nginx с stub_status
- установил prometheus с помощью оператора
- создал deployment с кастомным nginx образом и сайд-контейнером nginx-prometheus-exporter
- создал service и servicemonitor

```shell
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/master/bundle.yaml
kubectl get crds
kubectl get deploy 
kubectl apply -f rbac.yaml
kubectl describe clusterrolebinding prometheus
kubectl apply -f prometheus.yaml
kubectl port-forward svc/prometheus-operated 9090:9090
kubectl apply -f service-monitor.yaml
```