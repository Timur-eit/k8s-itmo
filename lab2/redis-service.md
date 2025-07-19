Headless Service с именем redis
Находится в namespace redis
Проксирует трафик к pod’ам с label app=redis
Работает на порту 6379, не имеет собственного IP
Позволяет DNS-имена вроде redis-0.redisы

Headless Service (clusterIP: None) чтобы у каждого pod’а был собственный DNS-адрес. Нужно для StatefulSet, чтобы обеспечить устойчивую идентификацию pod’ов по имени.
