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



## kubernetes-operators

- создал CustomResource и CustomResourceDefinition для mysql оператора
- создал часть логики mysql оператора при помощи python KOPF
- собрал образ, запушил его в docker hub и сделал деплой оператора
- исправил ошибки в mysql-operator.py


```shell
kubectl get jobs
NAME                         COMPLETIONS   DURATION   AGE
backup-mysql-instance-job    1/1           5s         3m4s
restore-mysql-instance-job   1/1           20s        2m25s
```

```shell
kubectl exec -it $MYSQLPOD -- mysql -potuspassword -e "select * from test;" otus-database
mysql: [Warning] Using a password on the command line interface can be insecure.
+----+-------------+
| id | name        |
+----+-------------+
|  1 | some data   |
|  2 | some data-2 |
+----+-------------+

```