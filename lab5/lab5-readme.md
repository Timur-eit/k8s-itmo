## Lab5

### Создание своего Helm-чарт для redis и chat-app из lab2 и lab3

#### Этапы выполнения

(1)
Генерация базовой структуру чарта  
`helm create redis-chat`

(2) Очистка шаблонов:  
Удалены все файлы не относящиеся

(3)
Создание и настройка values.yaml (значения из манифестов lab 2 и 3)

(4)
Описание шаблонов:

```
redis-statefulset.yaml
redis-service.yaml
chat-deployment.yaml
chat-service.yaml
```

(5) Очистка старых ресурсов (если были):

```bash
kubectl delete statefulset redis -n redis
kubectl delete service redis -n redis
kubectl delete deployment redis-chat-app -n redis-chat-app
kubectl delete service redis-chat-app -n redis-chat-app
```

(6) Установка чарта

```bash
helm upgrade --install redis-chat . \
  --create-namespace \
  --namespace redis-chat-app
```

(7) Проверка

```bash
kubectl get all -n redis
kubectl get all -n redis-chat-app

helm list -n redis-chat-app
helm history redis-chat -n redis-chat-app
```

web интерфейс http://localhost:30010 открывается

Примечания:  
`./charts/` удалил так как мой чарт не используется зависимости
