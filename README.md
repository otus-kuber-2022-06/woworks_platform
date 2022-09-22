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

## kubernetes-gitops
- установил flux и flagger в google cloud k8s cluster
- инфраструктурный репозиторий - https://gitlab.com/otus-kuber-artiom/microservices-demo
- вывод комманды **kubectl get canaries -n microservices-demo**
```shell
aslastin@aslastin-u7510:~$ kubectl get canaries -n microservices-demo
W0911 11:48:44.815455   20296 gcp.go:120] WARNING: the gcp auth plugin is deprecated in v1.22+, unavailable in v1.25+; use gcloud instead.
To learn more, consult https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
NAME       STATUS      WEIGHT   LASTTRANSITIONTIME
frontend   Succeeded   0        2022-09-08T22:10:27Z
```
- вывод команды **kubectl describe canary -n microservices-demo frontend**
```shell
aslastin@aslastin-u7510:~$ kubectl describe canary -n microservices-demo frontend
W0911 12:01:00.217068   22931 gcp.go:120] WARNING: the gcp auth plugin is deprecated in v1.22+, unavailable in v1.25+; use gcloud instead.
To learn more, consult https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
Name:         frontend
Namespace:    microservices-demo
Labels:       app.kubernetes.io/managed-by=Helm
Annotations:  helm.fluxcd.io/antecedent: microservices-demo:helmrelease/frontend
              meta.helm.sh/release-name: frontend
              meta.helm.sh/release-namespace: microservices-demo
API Version:  flagger.app/v1beta1
Kind:         Canary
Metadata:
  Creation Timestamp:  2022-09-08T21:42:42Z
  Generation:          3
  Managed Fields:
    API Version:  flagger.app/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        .:
        f:analysis:
          .:
          f:interval:
          f:maxWeight:
          f:stepWeight:
        f:progressDeadlineSeconds:
        f:service:
          .:
          f:gateways:
          f:hosts:
          f:port:
          f:retries:
            .:
            f:attempts:
            f:perTryTimeout:
            f:retryOn:
          f:targetPort:
          f:trafficPolicy:
            .:
            f:tls:
              .:
              f:mode:
        f:targetRef:
          .:
          f:apiVersion:
          f:kind:
          f:name:
    Manager:      kubectl-create
    Operation:    Update
    Time:         2022-09-08T21:42:42Z
    API Version:  flagger.app/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:status:
        .:
        f:canaryWeight:
        f:conditions:
        f:failedChecks:
        f:iterations:
        f:lastAppliedSpec:
        f:lastPromotedSpec:
        f:lastTransitionTime:
        f:phase:
        f:trackedConfigs:
    Manager:      flagger
    Operation:    Update
    Subresource:  status
    Time:         2022-09-08T21:43:27Z
    API Version:  flagger.app/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:meta.helm.sh/release-name:
          f:meta.helm.sh/release-namespace:
        f:labels:
          .:
          f:app.kubernetes.io/managed-by:
    Manager:      helm
    Operation:    Update
    Time:         2022-09-08T22:05:45Z
    API Version:  flagger.app/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:spec:
        f:analysis:
          f:threshold:
    Manager:      helm-operator
    Operation:    Update
    Time:         2022-09-08T22:06:45Z
    API Version:  flagger.app/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          f:helm.fluxcd.io/antecedent:
    Manager:         kubectl-annotate
    Operation:       Update
    Time:            2022-09-08T22:06:48Z
  Resource Version:  4492249
  UID:               865082b7-f354-4b36-a1a0-4bb74d160ecc
Spec:
  Analysis:
    Interval:                 20s
    Max Weight:               30
    Step Weight:              10
    Threshold:                7
  Progress Deadline Seconds:  60
  Service:
    Gateways:
      frontend
    Hosts:
      *
    Port:  80
    Retries:
      Attempts:         3
      Per Try Timeout:  1s
      Retry On:         gateway-error,connect-failure,refused-stream
    Target Port:        8080
    Traffic Policy:
      Tls:
        Mode:  DISABLE
  Target Ref:
    API Version:  apps/v1
    Kind:         Deployment
    Name:         frontend
Status:
  Canary Weight:  0
  Conditions:
    Last Transition Time:  2022-09-11T08:55:27Z
    Last Update Time:      2022-09-11T08:55:27Z
    Message:               Canary analysis completed successfully, promotion finished.
    Reason:                Succeeded
    Status:                True
    Type:                  Promoted
  Failed Checks:           0
  Iterations:              0
  Last Applied Spec:       7455d6df44
  Last Promoted Spec:      7455d6df44
  Last Transition Time:    2022-09-11T08:55:27Z
  Phase:                   Succeeded
  Tracked Configs:
Events:
  Type     Reason  Age                    From     Message
  ----     ------  ----                   ----     -------
  Normal   Synced  8m14s (x2 over 2d10h)  flagger  New revision detected! Scaling up frontend.microservices-demo
  Warning  Synced  7m54s (x2 over 2d10h)  flagger  canary deployment frontend.microservices-demo not ready: waiting for rollout to finish: 0 of 1 (readyThreshold 100%) updated replicas are available
  Normal   Synced  7m34s (x3 over 2d10h)  flagger  Starting canary analysis for frontend.microservices-demo
  Normal   Synced  7m34s (x3 over 2d10h)  flagger  Advance frontend.microservices-demo canary weight 10
  Normal   Synced  7m14s (x3 over 2d10h)  flagger  Advance frontend.microservices-demo canary weight 20
  Normal   Synced  6m54s (x2 over 2d10h)  flagger  Advance frontend.microservices-demo canary weight 30
  Normal   Synced  6m34s (x2 over 2d10h)  flagger  Copying frontend.microservices-demo template spec to frontend-primary.microservices-demo
  Warning  Synced  6m14s (x2 over 2d10h)  flagger  frontend-primary.microservices-demo not ready: waiting for rollout to finish: 1 old replicas are pending termination
  Normal   Synced  5m54s (x2 over 2d10h)  flagger  Routing all traffic to primary
  Normal   Synced  5m34s (x2 over 2d10h)  flagger  Promotion completed! Scaling down frontend.microservices-demo

```

## kubernetes-logging
- поднял кластер в gcp: 1 нода в дефолт пуле, 3 ноды в пуле infra
- поднял HipsterShop 
- установил elasticsearch, kibana и fluent-bit
- установил ingress-controller
- настроил отправку логов с подов ingress-controller-а в elastic c помощью fluent-bit
- установил prometheus-operator для мониторинга elasticsearch
- установил elasticsearch-exporter
- добился отображения в Kibana логов nginx-ingress
- настроил отображение запросов с разными статусами из nginx-ingress в разных графиках на одном dashboard
- установил loki и promtail
- настроил datasource loki в Grafana
- создал Dashboard, на котором одновременно выведем метрики nginx-ingress, количество запросов к nginx-ingress в секунду и его логи