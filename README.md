# woworks_platform
woworks Platform repository

- Был создан Dockerfile для nginx, который показывает статические файлы из папки /app
- Был создан k8s манифест для образа созданного в предыдущем пункте. initContainers секция содержит скрипт который генерирует index.html в папке /app
- Был создан frontend-pod-healthy.yaml манифест, который запускает frontend образ. В манифесте установлено значение для переменной окружения PRODUCT_CATALOG_SERVICE_ADDR