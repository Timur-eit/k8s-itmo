### Задача

Задеплоить простенькое приложение и подключить к нему базу данных redis из прошлой лабораторной работы
Образ брать отсюда https://hub.docker.com/r/rayhimself/basic-redis-chat-app-api

URL для подключения указывается в переменной окружения REDIS_ENDPOINT_URL, лучше всего указать там внутренне днс имя сервиса, который вы создавали в первой лаботаторке для redis кластераПо итогу должно получиться что-то такое (web интерефейс учебного приложения – чат)

Также можно подключиться к базе данных редис и убедиться что там появились конекты

### Ход выполнения

1. Создаем новый нс redis-chat-app  
   `kubectl create namespace redis-chat-app`

2. Создали 2 манифеста для деплоймента и сервиса  
   `redis-chat-app-deployment.yaml`  
   и  
   `redis-chat-app-service.yaml`

3) Применили и проверили

`kubectl apply -f redis-chat-app-deployment.yaml`  
`kubectl apply -f redis-chat-app-service.yaml`

`kubectl get pods -n default redis-chat-app`  
`kubectl get svc redis-chat-app`

4. Отладка и тестирование:  
   При обращении к web интерфесу (http://localhost:30010/) была ошибка.
   Логи через `kubectl logs redis-chat-app-f4b4bdbcd-9vlt5 -n redis-chat-app` и проверка в `kubectl describe svc redis-chat-app -n redis-chat-app` показали, что ошибка в манифестах при указании `containerPort` (в Deployment) и в `targetPort` (в Service), нужно было указать 8080.

Итог:  
web интерфейс заработал, команда `MONITOR` внутри контейнера (`kubectl exec -it redis-0 -n redis -- redis-cli`\*) подтверждает активность и работу приложения

\*)
Выполнить `redis-cli` в контейнере, который запущен внутри
пода с именем `redis-0` (Pod из StatefulSet), в namespace redis
