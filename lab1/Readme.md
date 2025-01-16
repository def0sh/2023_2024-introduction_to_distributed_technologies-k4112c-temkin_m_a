## Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."

University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)

Year: 2023/2024

Group: K4112

Author: Temkin Maksim Aleksandrovich

Lab: Lab2

Date of create: 08.12.2024

Date of finished: 16.01.2024

**1. Развернуть minikube cluster**

```
$ minikube start
```
**2. Манифест файла для развертывания vault**

```
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    app: vault
spec:
  containers:
  - name: vault
    image: hashicorp/vault:latest
    ports:
    - containerPort: 8200
```

**apiVersion:** v1
Версию API Kubernetes

**kind:** Pod
Тип создаваемого объекта.

**metadata**
Содержит метаинформацию о Pod

**name:** vault — имя  используется для идентификации

**labels:** app: vault — метки  для классификации Pod

**spec**
Описывает спецификацию Pod

**containers** — список контейнеров, которые будут запущены в Pod.

**image:** — образ контейнера  

**name:** имя контейнера.

**ports** открытые порты

**name:** vault-port — название порта

**containerPort:** 8200 — порт внутри контейнера, который будет доступен для работы 

**3. Создание или обновление объекта в Pod**
```
$ kubectl apply -f vault.yaml
```
**4. Создаёт объект Service, который открывает доступ к Pod vault через порт 8200**
```
$ minikube kubectl -- expose pod vault --type=NodePort --port=8200
```
**5. Перенаправление локального порта 8200 на порт 8200 сервиса vault для получения доступа к сервису**
```
$ minikube kubectl -- port-forward service/vault 8200:8200
```
**6. Перейти на url**
```
http://localhost:8200/ui/vault/dashboard
```
**7. Найти токен в логах**
```
$ minikube kubectl logs vault
```



