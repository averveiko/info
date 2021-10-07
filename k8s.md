Единый конспект на базе курса https://github.com/Slurmio/school-dev-k8s

# Pod

## 1. Создаем Pod

Для этого выполним команду:

```bash
kubectl apply -f ~/school-dev-k8s/practice/2.application-abstractions/1.pod/pod.yaml
```

Проверим результат, для чего выполним команду:

```bash
kubectl get pod
```

## 2. Получаем информцию о Pod
```bash
kubectl describe pod my-pod
```

## 3. Чистим за собой кластер

```bash
kubectl delete pod --all
```

# ReplicaSet

## 1. Создаем Replicaset

Для этого выполним команду:

```bash
kubectl apply -f ~/school-dev-k8s/practice/2.application-abstractions/2.replicaset/replicaset.yaml
```

## 2. Скейлим Replicaset

Для этого выполним команду:

```bash
kubectl scale replicaset my-replicaset --replicas 3
```

## 3. Удаляем один из Pod

Для этого выполним команду подставив имя своего Pod:

> можно воспользоваться автоподстановкой по TAB

```bash
kubectl delete pod my-replicaset-pbtdm
```

## 4. Добавляем в Replicaset лишний Pod

Открываем файл `2.replicaset/pod.yaml`

```bash
vim ~/school-dev-k8s/practice/2.application-abstractions/2.replicaset/pod.yaml
```

И в него после metadata: на следующей строке добавляем:

```yaml
  labels:
    app: my-ap
```

В итоге должно получиться:

```yaml
.............
kind: Pod
metadata:
  name: my-pod
  labels:
    app: my-app
spec:
.............
```

Сохраняем и выходим.

Создаем дополнительный Pod, для этого выполним команду:

```bash
kubectl apply -f ~/school-dev-k8s/practice/2.application-abstractions/2.replicaset/pod.yaml
```

Проверяем результат, для этого выполним команду:

```bash
kubectl get pod
```

Результат должен быть примерно следующим:

```bash
NAME                  READY     STATUS        RESTARTS   AGE
my-pod                0/1       Terminating   0          1s
my-replicaset-55qdj   1/1       Running       0          3m
my-replicaset-pbtdm   1/1       Running       0          8m
my-replicaset-szqgz   1/1       Running       0          6m
```

## 5. Обновляем версию Image

Для этого выполним команду:

```bash
kubectl set image replicaset my-replicaset nginx=nginx:1.13
```

Проверяем результат, для этого выполним команду:

```bash
kubectl get pod
```

Результат должен быть примерно следующим:

```bash
NAME                  READY     STATUS        RESTARTS   AGE
my-replicaset-55qdj   1/1       Running       0          3m
my-replicaset-pbtdm   1/1       Running       0          8m
my-replicaset-szqgz   1/1       Running       0          6m
```

И проверяем сам Replicaset, для чего выполним команду:

```bash
kubectl describe replicaset my-replicaset
```

В результате находим строку Image и видим:

```bash
  Containers:
   nginx:
    Image:        nginx:1.13
```

Проверяем версию image в pod. Для этого выполним команду, подставив имя своего Pod

```bash
kubectl describe pod my-replicaset-55qdj
```

Видим что версия имаджа в поде не изменилась:

```bash
  Containers:
   nginx:
    Image:        nginx:1.12
```

Помогаем поду обновиться - для этого выполним команду, подставив имя своего Pod

```bash
kubectl delete po my-replicaset-55qdj
```

Проверяем результат, для этого выполним команду:

```bash
kubectl get pod
```

Результат должен быть примерно следующим:

```bash
NAME                  READY     STATUS              RESTARTS   AGE
my-replicaset-55qdj   0/1       Terminating         0          11m
my-replicaset-cwjlf   0/1       ContainerCreating   0          1s
my-replicaset-pbtdm   1/1       Running             0          16m
my-replicaset-szqgz   1/1       Running             0          14m
```

Проверяем версию Image в новом Pod. Для этого выполним команду,
подставив имя своего Pod

```bash
kubectl describe pod my-replicaset-cwjlf
```

Результат должен быть примерно следующим:

```bash
    Image:          nginx:1.13
```

## 6. Чистим за собой кластер

Для этого выполним команду:

```bash
kubectl delete replicaset --all
```
# Deployment

## 1. Создаем deployment

Для этого выполним команду:

```bash
kubectl apply -f ~/school-dev-k8s/practice/2.application-abstractions/3.deployment/
```

Проверяем список pods, для этого выполним команду:

```bash
kubectl get pod
```

Результат должен быть примерно таким:

```bash
NAME                             READY     STATUS              RESTARTS   AGE
my-deployment-7c768c95c4-47jxz   0/1       ContainerCreating   0          2s
my-deployment-7c768c95c4-lx9bm   0/1       ContainerCreating   0          2s
```

Проверяем список replicaset, для этого выполним команду:

```bash
kubectl get replicaset
```

Результат должен быть примерно таким:

```bash
NAME                       DESIRED   CURRENT   READY     AGE
my-deployment-7c768c95c4   2         2         2         1m
```

## 2. Обновляем версию image

Обновляем версию image для container в deployment my-deployment.
Для этого выполним команду:

```bash
kubectl set image deployment my-deployment nginx=nginx:1.13
```

Проверяем результат, для этого выполним команду:

```bash
kubectl get pod
```

Результат должен быть примерно таким:

```bash
NAME                             READY     STATUS              RESTARTS   AGE
my-deployment-685879478f-7t6ws   0/1       ContainerCreating   0          1s
my-deployment-685879478f-gw7sq   0/1       ContainerCreating   0          1s
my-deployment-7c768c95c4-47jxz   0/1       Terminating         0          5m
my-deployment-7c768c95c4-lx9bm   1/1       Running             0          5m
```

И через какое-то время вывод этой команды станет:

```bash
NAME                             READY     STATUS    RESTARTS   AGE
my-deployment-685879478f-7t6ws   1/1       Running   0          33s
my-deployment-685879478f-gw7sq   1/1       Running   0          33s
```

Проверяем что в новых pod новый image. Для этого выполним команду,
заменив имя pod на имя вашего pod:

```bash
kubectl describe pod my-deployment-685879478f-7t6ws
```

Результат должен быть примерно таким:

```bash
    Image:          nginx:1.13
```

Проверяем что стало с replicaset, для этого выполним команду:

```bash
kubectl get replicaset
```

Результат должен быть примерно таким:

```bash
NAME                       DESIRED   CURRENT   READY     AGE
my-deployment-685879478f   2         2         2         2m
my-deployment-7c768c95c4   0         0         0         7m
```

## 3. Чистим за собой кластер

```bash
kubectl delete deployment --all
```

# Resources

## 1. Создаем deployment с ресурсами

```bash
kubectl apply -f ~/school-dev-k8s/practice/2.application-abstractions/4.resources/
```

Смотрим что получилось

```bash
kubectl get pod
```

Должны увидеть что-то типа такого

```bash
NAME                             READY     STATUS              RESTARTS   AGE
my-deployment-57fff9c845-2qv5l   0/1       ContainerCreating   0          1s
my-deployment-57fff9c845-h8bbw   0/1       ContainerCreating   0          1s
```

## 2. Увеличиваем количество ресурсов для нашего деплоймента

```bash
kubectl patch deployment my-deployment --patch '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","resources":{"requests":{"cpu":"10"},"limits":{"cpu":"10"}}}]}}}}'
```

Смотрим что получилось

```bash
kubectl get pod
```

Должны увидеть что-то типа такого

```bash
NAME                             READY   STATUS    RESTARTS   AGE
my-deployment-68684546b9-gm894   0/1     Pending   0          37s
my-deployment-9ddd794f9-jvff6    1/1     Running   0          51s
my-deployment-9ddd794f9-ztvq9    1/1     Running   0          51s
```

Смотрим, почему поды не могут создаться

```bash
kubectl describe po my-deployment-845d88fdcf-9bd29
```

Видим в эвентах что-то типа

```bash
Events:
  Type     Reason            Age               From               Message
  ----     ------            ----              ----               -------
  Warning  FailedScheduling  87s   default-scheduler  0/8 nodes are available: 8 Insufficient cpu.
```

## 3. Чистим за собой кластер

```bash
kubectl delete deployment --all
```
