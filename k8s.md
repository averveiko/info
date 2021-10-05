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
