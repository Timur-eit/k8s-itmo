StatefulSet redis, в namespace redis  
Один pod: redis-0  
Отдельный том redis-data-redis-0 объёмом 1Gi, примонтированный в /data.
Контейнер Redis, работающий на порту 6379  
Связь через headless-сервис redis
