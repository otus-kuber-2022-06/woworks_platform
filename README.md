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
