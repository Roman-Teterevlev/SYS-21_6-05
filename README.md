# SYS-21_6-05
Kubernetes. Часть 1
## Задание 1
Выполните действия:
1. Запустите Kubernetes локально, используя k3s или minikube на свой выбор.
2. Добейтесь стабильной работы всех системных контейнеров.
3. В качестве ответа пришлите скриншот результата выполнения команды kubectl get po -n kube-system.
### Ответ:
<img width="960" alt="6-05_1 1" src="https://github.com/Roman-Teterevlev/SYS-21_6-05/assets/132853752/0de97edb-a817-45f0-9e7b-edcab683427f">

## Задание 2
Есть файл с деплоем:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis
        env:
         - name: REDIS_PASSWORD
           value: password123
        ports:
        - containerPort: 6379
```
Выполните действия:
1. Измените файл с учётом условий:
- redis должен запускаться без пароля;
- создайте Service, который будет направлять трафик на этот Deployment;
- версия образа redis должна быть зафиксирована на 6.0.13.
2. Запустите Deployment в своём кластере и добейтесь его стабильной работы.
3. В качестве решения пришлите получившийся файл.
### Ответ:
```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis:6.0.13
        env:
         - name: ALLOW_EMPTY_PASSWORD
           value: "yes"
        ports:
         - containerPort: 6379
---
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
```

<img width="960" alt="6-05_2 1" src="https://github.com/Roman-Teterevlev/SYS-21_6-05/assets/132853752/58a39990-b84f-4df8-8a5a-b9888af61846">
<img width="960" alt="6-05_2 2" src="https://github.com/Roman-Teterevlev/SYS-21_6-05/assets/132853752/9ca05c89-fd9c-436d-b184-564566dd90d6">

## Задание 3
Выполните действия:
1. Напишите команды kubectl для контейнера из предыдущего задания:
- выполнения команды ps aux внутри контейнера;
- просмотра логов контейнера за последние 5 минут;
- удаления контейнера;
- проброса порта локальной машины в контейнер для отладки.
2. В качестве решения пришлите получившиеся команды.
### Ответ:
```
kubectl exec -it redis-58c6d4947b-m9wwx -- ps aux
```
```
kubectl logs --since=5m redis-58c6d4947b-m9wwx
```
```
kubectl delete -f redis.yaml
```
```
kubectl port-forward pod/redis-58c6d4947b-m9wwx 53:6379
```

<img width="960" alt="6-05_3 1" src="https://github.com/Roman-Teterevlev/SYS-21_6-05/assets/132853752/0f7811e0-53ff-474d-b21b-02a53df83580">
<img width="960" alt="6-05_3 2" src="https://github.com/Roman-Teterevlev/SYS-21_6-05/assets/132853752/709a0f51-bf4e-4637-99a3-3b64c44360a1">
