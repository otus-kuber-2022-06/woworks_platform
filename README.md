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

## kubernetes-vault
- вывод helm status vault
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ helm status vault
NAME: vault
LAST DEPLOYED: Fri Sep 16 01:09:52 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
Thank you for installing HashiCorp Vault!

Now that you have deployed Vault, you should look over the docs on using
Vault with Kubernetes available here:

https://www.vaultproject.io/docs/


Your release is named vault. To learn more about the release, try:

  $ helm status vault
  $ helm get manifest vault

```  
- инициализация vault-а
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ kubectl exec -it vault-0 -- vault operator init --key-shares=1 --key-threshold=1
Unseal Key 1: puN2VGd/reVCtZ5kwMKRoXeF3EZQFImDwtoO/dyLqAk=

Initial Root Token: hvs.v6TwSkKJaVo3Mzgu3W2WzLlQ

Vault initialized with 1 key shares and a key threshold of 1. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 1 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated root key. Without at least 1 keys to
reconstruct the root key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.

```
- вывод статуса vault-а после распечатывания подов
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ kubectl exec -it vault-0 -- vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.11.3
Build Date      2022-08-26T10:27:10Z
Storage Type    consul
Cluster Name    vault-cluster-3431bbb2
Cluster ID      7afad71d-40d7-919f-f19a-ab347d8f5356
HA Enabled      true
HA Cluster      https://vault-0.vault-internal:8201
HA Mode         active
Active Since    2022-09-15T22:16:29.100828563Z

```
- логин в vault
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ kubectl exec -it vault-0 -- vault login
Token (will be hidden): 
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                hvs.v6TwSkKJaVo3Mzgu3W2WzLlQ
token_accessor       UckqOygf2XXwIF7XzNWGzWdS
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]


```

- список авторизаций
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ kubectl exec -it vault-0 -- vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_7d279ed8    token based credentials
```

- чтение секрета
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ kubectl exec -it vault-0 -- vault read otus/otus-ro/config
Key                 Value
---                 -----
refresh_interval    768h
password            asajkjkahs
username            otus

```

- обновленный список авторизаций
```shell
aslastin@aslastin-u7510:~/Documents/otus/vault-helm$ kubectl exec -it vault-0 -- vault auth list
Path           Type          Accessor                    Description
----           ----          --------                    -----------
kubernetes/    kubernetes    auth_kubernetes_d9d25381    n/a
token/         token         auth_token_7d279ed8         token based credentials

```

- Почему мы смогли записать otus-rw/config1 но не смогли otus-rw/config
не хватает capability "update" для пути "otus/otus-rw/*"
```shell
path "otus/otus-rw/*" {
    capabilities = ["read", "create", "update", "list"]
}
```

- создание и отзыв сертификатов

```shell
aslastin@aslastin-u7510:~/IdeaProjects/woworks_platform/kubernetes-vault$ kubectl exec -it vault-0 -- vault write pki_int/issue/example-dot-ru common_name="gitlab.example.ru" ttl="24h"
W0918 00:03:32.745287  464110 gcp.go:120] WARNING: the gcp auth plugin is deprecated in v1.22+, unavailable in v1.25+; use gcloud instead.
To learn more, consult https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUKFb6JkRnFt/J8GzAASLtGquvhUIwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMjA5MTcyMDU4NDNaFw0yNzA5
MTYyMDU5MTNaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANUOPRzdjnRu
/1mUMgoTNZnU0iJm1SgWoFgXgdJVOy3bn9nbU6zOpZSPq8ApByBNH4tjbvgB65Bf
dZZJQXPA2vaDGI80Ttg4GclTuaJTAQQJS6Akl/Swn2PzERhfc6vNo36zYARZzDCX
SR1lXhK3xWsXFDR/I0LEvzhsLgvhkKjcr/eorvyppsavbilB3FBmVceGf8n+0SXK
vW5D6GAoVtqx3tmAIOK9AajW/tTI+N4nZNgaQhI3X2MaB/a74d8H5VJMwZUjai5t
sYCRAXVwsKJ64O7A34zq8SFED5aU0akCddl5kg6SC+vIZegPK1gp8cWQAIsA9OVL
19Bxn3RRa9ECAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQU/SegNWcyA3PofWs+NHeQDbFmzKwwHwYDVR0jBBgwFoAU
pe/AeDfFpJ1GDjbqPvdZzn7mvxIwNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
ErHE0Te57PWvQFwHgJ15F9s/pN28tm8FdCqvlKQfiU9gXEhz1vhH72u9D11HiaCS
BF7MGeaCeDLLgMIrbSwfioape/MtgAwjXWiLsPYjdGCGmtaDif2+3IhCi5UTLGvP
C/1zTmvhhi8EdrWxM6wWHPkj77VGX4mJ6eGd1dZaCBBibdFPKmUV1KHUQpy8eC8a
YZusM3Q5PSmkWxIgEXEavB9yGzgVsR2BOx3QLUTotzRAeVoBb4/u022YVNWdAaFk
Eze0o+0D4u9TYELbR8AiMHuO/08mqOY/BH70iDwnv/q4knLgNhKoolUUmV5fuMu2
/44JFsPH+NbYc+fW4U3QNw==
-----END CERTIFICATE----- -----BEGIN CERTIFICATE-----
MIIDMjCCAhqgAwIBAgIUAUMfXJNU6fi2no9/GZ8qWb42HrMwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMjA5MTcyMDUyMzhaFw0zMjA5
MTQyMDUzMDhaMBUxEzARBgNVBAMTCmV4bWFwbGUucnUwggEiMA0GCSqGSIb3DQEB
AQUAA4IBDwAwggEKAoIBAQCs6H/W8+Xg2YD5L20WZt1FL/9GYHutq5yY6l8+V6Ot
/C+McSXdHAOBwSEeY46P8lap/dDTq4x4FjvZjAqA8gNqVYRPYS+DYR5U8w4AjBti
oXaYeF+uI9yHRVzrJBP0U2XUUW01Cmsuz4Y0kOQtMqzEE2Gaw3TMDmMSuUNs2geS
ohBmSsK2bsoZAL/tKpxeeXIHsp2WODaPQlaTanf3ozeHcwwuqNakuxZK24aIS3lc
t2y9ajNnYJDWVHXnIaY9bgHbBu2l1SMQOh/9UQX22zZXoDThULvLVFQXnap8+7gQ
gcPqZjRQJHT5MdGTYppGdOgpEDr7LYOUno75j82PlB4JAgMBAAGjejB4MA4GA1Ud
DwEB/wQEAwIBBjAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBSl78B4N8WknUYO
Nuo+91nOfua/EjAfBgNVHSMEGDAWgBSl78B4N8WknUYONuo+91nOfua/EjAVBgNV
HREEDjAMggpleG1hcGxlLnJ1MA0GCSqGSIb3DQEBCwUAA4IBAQCjarXZDcpTZsSM
duGAPPw0vwXtDmQ3tv97X8LK06N1yyLqtTPgslHU+h/kryQWmBZrKbPeLfNHUw9T
suz9/dCB/qx4998LMSH8WWe8osIhjepIIRFR0GJ944R+Y4705c/TKpqzbjn2l0d3
I46Xg+uctTE7ATi+ek+iagQOjZrdp869vykmSVh8N+7q1EEcghdAsBmMdPfTZnq5
AI1d85Swdg9f9UNQTVcVjj6YjqR4Qq4P8h95tUJrc+rmo6Sk1/6j2lijhsSw0QsV
PSMwKJfiUtQgCb/lHf9CCA3r7E5Oq1TIJUG0/1kxWAI8W+N8pQsK6JRwj2fFQ01L
qi+P5DIm
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZzCCAk+gAwIBAgIUM/CjYeBrLcTBFrQAfnfoiTPdDFAwDQYJKoZIhvcNAQEL
BQAwLDEqMCgGA1UEAxMhZXhhbXBsZS5ydSBJbnRlcm1lZGlhdGUgQXV0aG9yaXR5
MB4XDTIyMDkxNzIxMDMwNVoXDTIyMDkxODIxMDMzNFowHDEaMBgGA1UEAxMRZ2l0
bGFiLmV4YW1wbGUucnUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDG
L6wro+gbCfIEUbZGagEe6moqqlkXb5354oSQPGidAF+qzREkiMHm7tr+V6de480B
aozF/ZSYvt2PP0DgpW5fOYb73TToEFOvqXWMwUAznzZMOV9AL1QL8fSrsqwilcsN
j40rCPf6ddE9mzGXrkjnNcVttTM6tjTkDcVFlGy5MIrpu7/9Ylj1rZaZDvl0Vh52
f2DU/WiRB+iTk3pokXYhGvSmAIGXdc4DeN5MaPtLi0ITt2Y7nWDnOo127K4SbmUU
gSeVygMp0ZNhUK4dwYhixElfgcq3vionJk/DAGMzBkhYXn4GDl92YqQslcCLS/rz
rL0LJ5BJt2VFrsc+R4PHAgMBAAGjgZAwgY0wDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQU8yFQgz0CfxkNclIi
RD/lzE7cZVswHwYDVR0jBBgwFoAU/SegNWcyA3PofWs+NHeQDbFmzKwwHAYDVR0R
BBUwE4IRZ2l0bGFiLmV4YW1wbGUucnUwDQYJKoZIhvcNAQELBQADggEBAJKdJOQ7
ti+EOx4CyOmmfukUxYJv+D7VWMfBGMP8FZNihgnGIFAzNdBH8BxIzL1tKD63tChv
CfgKKZAOAVUj7GRmFgdHJK69jZcmeYjH2kQxpLyCyGW5RNa4fA7CiLs6CBnw5VKl
GyhWYBvYmln4x3PVIAa2vf3OXcQg22DE4kzmu+GVg9WD0PWQzFhD5H2AytjVZ1eV
1+sCyyUC7CRQxjUj+RvUod1eAWsAqb+2pnUGpxtyGsTQMRM7xoR91AKTcBWMMjec
qSRTN0txqs1qCQ1UZfxSKNeP5eeZNEqb60dzGzbFDa9Ob2AGmlkEywj63sjddsGH
3FZCylZHRugg7vE=
-----END CERTIFICATE-----
expiration          1663535014
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUKFb6JkRnFt/J8GzAASLtGquvhUIwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMjA5MTcyMDU4NDNaFw0yNzA5
MTYyMDU5MTNaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANUOPRzdjnRu
/1mUMgoTNZnU0iJm1SgWoFgXgdJVOy3bn9nbU6zOpZSPq8ApByBNH4tjbvgB65Bf
dZZJQXPA2vaDGI80Ttg4GclTuaJTAQQJS6Akl/Swn2PzERhfc6vNo36zYARZzDCX
SR1lXhK3xWsXFDR/I0LEvzhsLgvhkKjcr/eorvyppsavbilB3FBmVceGf8n+0SXK
vW5D6GAoVtqx3tmAIOK9AajW/tTI+N4nZNgaQhI3X2MaB/a74d8H5VJMwZUjai5t
sYCRAXVwsKJ64O7A34zq8SFED5aU0akCddl5kg6SC+vIZegPK1gp8cWQAIsA9OVL
19Bxn3RRa9ECAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQU/SegNWcyA3PofWs+NHeQDbFmzKwwHwYDVR0jBBgwFoAU
pe/AeDfFpJ1GDjbqPvdZzn7mvxIwNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
ErHE0Te57PWvQFwHgJ15F9s/pN28tm8FdCqvlKQfiU9gXEhz1vhH72u9D11HiaCS
BF7MGeaCeDLLgMIrbSwfioape/MtgAwjXWiLsPYjdGCGmtaDif2+3IhCi5UTLGvP
C/1zTmvhhi8EdrWxM6wWHPkj77VGX4mJ6eGd1dZaCBBibdFPKmUV1KHUQpy8eC8a
YZusM3Q5PSmkWxIgEXEavB9yGzgVsR2BOx3QLUTotzRAeVoBb4/u022YVNWdAaFk
Eze0o+0D4u9TYELbR8AiMHuO/08mqOY/BH70iDwnv/q4knLgNhKoolUUmV5fuMu2
/44JFsPH+NbYc+fW4U3QNw==
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAxi+sK6PoGwnyBFG2RmoBHupqKqpZF2+d+eKEkDxonQBfqs0R
JIjB5u7a/lenXuPNAWqMxf2UmL7djz9A4KVuXzmG+9006BBTr6l1jMFAM582TDlf
QC9UC/H0q7KsIpXLDY+NKwj3+nXRPZsxl65I5zXFbbUzOrY05A3FRZRsuTCK6bu/
/WJY9a2WmQ75dFYedn9g1P1okQfok5N6aJF2IRr0pgCBl3XOA3jeTGj7S4tCE7dm
O51g5zqNduyuEm5lFIEnlcoDKdGTYVCuHcGIYsRJX4HKt74qJyZPwwBjMwZIWF5+
Bg5fdmKkLJXAi0v686y9CyeQSbdlRa7HPkeDxwIDAQABAoIBADTapeoFjnMXlem8
+QWSY7YhAitavuPXKqRn2r60vZmRbfIZOocpybyS2uiV69cMnbHQ/rYWyLhrvp7j
WaQPun2+09b7yY5SPF9OrEDbkQmzXkG500NE2/9Lidr4jZuzozF8pUNi4u9y2Fi7
uOAg5OR0nLCDcKQpz7U6TvAdg1lntpaUMPkAFPgeFbbTB1cgBL1SMSxpajFZhQI/
CH1VdUSaZyjDIawiPPTcovlSOHUBUWSzvqWICrCyqQAhUwL4AqjN1OS+n82AGr+9
oc8eyLC0TyWvN8yFYwCA5rISt5GgIPB9vlPtvXlaRAflrJf+x/7TyMr22ljK+ae/
4wzTZ4ECgYEA29Av/Aoxl5AzWjhE7Bcu9OeLXFqdtzX937wAJwkitriUz11MacLz
y+a+XcB6LTzHcvxNVvLNYneRUBtERybV/omUTcuB1TZfD8EY8ipArwYkynBdsJSO
x+clmeq5dO8l5hQ2n8L3WIgWRmi1owsmlG6kzSJ5Wshkq4paaanuPOECgYEA5tAJ
NXJXEomrXUMLhZWGIqea5eLi1UJCvg7aQ8/zlkdWuOEWIjX/3lQDH9GMDuWFsyyM
QVvBbnmK7MI9lFnXxNP6J5YlT8ie8w/398Ip4Mfahn35GyqjP5jLUbQqjNQQ22rB
U8tV0BOX5SjTqHnbZ2750rEdcsOYlccGSRAybacCgYAaUovseqwFolcD7skSHJSa
7z9ZB+DrF6QXLsQ16Hont2LakimcYKQhPrh9MZdv419e/aelSd4edIMKPLmoa5Yk
rQT9a+lYRvKLSGxcU4L7cpjlRR9zDdcrSRrPm7QptJmtO65u40ILcAB63f8iTyuH
ithhWEd//49Om3rIPHkzAQKBgAwrBYFC0OJSTa1x/maBdnvXOCxMZMb3YktgvxL6
II5YnnLKsv8Et8OBzFReGLMkWIMUoyRC8RSg5bkLmHF8twpSXoq9uKPXIMIRonH2
U3zdLGEQvRQ4kDinFnjtNZ0NBQJ8Q0rmmlSI4+YZ2+ay3eU7DYKVAcduFlo3DtbF
OtNbAoGBALMrIs+/DGmiekm7ShDUH3Y+ZLQ1qD/oz5v2iVMCOW6sAyGZJ13hXY+T
MidFlQcPfuhehS9vBOPQW2tEArrpyFi1nkiXR2dbo1eZYYDSnGeJ1UlSJCl28ZQr
a0zLawofnAV31uFXP4mD5vf0LhsZIESEZ2QdSz8j8VbnkTFTHe+j
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       33:f0:a3:61:e0:6b:2d:c4:c1:16:b4:00:7e:77:e8:89:33:dd:0c:50
aslastin@aslastin-u7510:~/IdeaProjects/woworks_platform/kubernetes-vault$ kubectl exec -it vault-0 -- vault write pki_int/revoke serial_number="33:f0:a3:61:e0:6b:2d:c4:c1:16:b4:00:7e:77:e8:89:33:dd:0c:50"
W0918 00:04:08.910854  465663 gcp.go:120] WARNING: the gcp auth plugin is deprecated in v1.22+, unavailable in v1.25+; use gcloud instead.
To learn more, consult https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke
Key                        Value
---                        -----
revocation_time            1663448651
revocation_time_rfc3339    2022-09-17T21:04:11.0922095Z
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

## kubernetes-storage
- установил HostPath Driver Plugin в кластер с помощью https://github.com/kubernetes-csi/csi-driver-host-path
- создал StorageClass c именем csi-hostpath-sc
- создал PVC с именем storage-pvc, у которого storageClassName: csi-hostpath-sc
- создал Pod с который как Volume использует storage-pvc

## kubernetes-debug
- kubectl debug не нужно устанавливать с варсии 1.20, но это уже другой дебаг
- установил DaemonSet из agent_daemonset.yml c image: aylei/debug-agent:0.0.1
- убедился что strace -c -p1 выдает "trace: attach: ptrace(PTRACE_SEIZE, 1): Operation not permitted"
- пробовал менять image на latest и запускать с ключем "--agentless=false", но k8s-ый дебаг такое не умеет
- добавлял в манифест пода 
```yaml
securityContext:
  capabilities:
    add: [ "SYS_PTRACE" ]
```
 но так и не смог побороть 'ptrace(PTRACE_SEIZE, 1): Operation not permitted'
 - итак теоретически я знаю как победить ошибку, но практически не могу в gcp создать кластер с версией ниже 1.21
- установил netperf-operator
- протестировал скорость с помощью cr.yaml
- применил Calico NetworkPolicy которая блокирует поды Netperf и убедился, что тест не выполняется
- устанавливаем iptables-tailer (а также ClusterRole, ServiceAccount и ClusterRoleBinding для него)
- IPTABLES_LOG_PREFIX переменную выставляем в "calico-packet:"
- задаем JOURNAL_DIRECTORY - "/var/log/journal"
- меняем image тэг на virtualshuric/kube-iptables-tailer:8d4296a
- смотрим event-ы netperf-operator пода и видим "Warning PacketDrop" 