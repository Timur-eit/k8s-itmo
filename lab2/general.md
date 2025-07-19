(1)
2 файла
redis-statefulset.yaml для StatefulSet
и
redis-service.yaml для создания сервиса

StatefulSet (как Deployment) — контроллер, который управляет созданием и обновлением подов
В подах запускаются контейнеры с приложениями
Service — это сетевой объект для сетевого взаимодействия подов

(2) Работа в cli
➜ kubectl create namespace redis
namespace/redis created

➜ kubectl apply -f redis-service.yaml
service/redis created

➜ kubectl apply -f redis-statefulset.yaml
statefulset.apps/redis created

➜ kubectl get pods -n redis
NAME READY STATUS RESTARTS AGE
redis-0 0/1 ContainerCreating 0 9s

<!--ContainerCreating видимо из-за того что скачивал образ, в след уже показал Running  -->

➜ kubectl get pods -n redis
NAME READY STATUS RESTARTS AGE
redis-0 1/1 Running 0 3m43s

➜ kubectl get pvc -n redis
NAME STATUS VOLUME CAPACITY ACCESS MODES STORAGECLASS VOLUMEATTRIBUTESCLASS AGE
redis-data-redis-0 Bound pvc-52effcd8-2698-418f-8c08-de255e3395ab 1Gi RWO hostpath <unset> 18s

➜ kubectl get svc -n redis
NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
redis ClusterIP None <none> 6379/TCP 30s

Для работы внутри контейнера
kubectl exec -it redis-0 -n redis -- redis-cli
`--` для разделения аргументов kubectl и redis-cli

127.0.0.1:6379> set testKey tesData
OK
127.0.0.1:6379> get testKey
"tesData"
127.0.0.1:6379>

(3)
kubectl describe pod redis-0 -n redis | grep -i image
Поменял с latest на 7.4.5-alpine в манифесте
kubectl delete pod redis-0 -n redis
➜ kubectl apply -f redis-statefulset.yaml
statefulset.apps/redis configured

➜ kubectl describe pod redis-0 -n redis | grep -i image
Image: redis:7.4.5-alpine
Image ID:
Normal Pulling 2s kubelet Pulling image "redis:7.4.5-alpine"
