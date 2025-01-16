## Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."

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
**2. Манифест файла для развертывания deployment-react**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-react
spec:
  replicas: 2
  selector:
    matchLabels:
      app: react
  template:
    metadata:
      labels:
        app: react
    spec:
      containers:
        - image: ifilyaninitmo/itdt-contained-frontend:master
          name: react
          ports:
          - name: react-port
            containerPort: 3000
          env:
            - name: REACT_APP_USERNAME
              value: 'test_app'
            - name: REACT_APP_COMPANY_NAME
              value: 'test_company'
```

**replicas:** 
Количество экземпляров приложения, которые должны быть запущены одновременно

**selector:** 
Определяет, какие поды будут управляться этим деплойментом

**3. Манифест файла для Service**
```
apiVersion: v1
kind: Service
metadata:
  name: front-service
spec:
  selector:
    app: react
  type: NodePort
  ports:
    - port: 3010
      name: react-port
      targetPort: 3000
      protocol: TCP
```
**4. Привязать файл манифеста**
```
$ kubectl apply -f service.yaml
```
**5. Перенаправление запрос с Service на pods**
```
$ minikube kubectl -- port-forward service/frontend-service 3010:3010
```

**6. Перейти в браузере на url**
```
http://localhost:3010
```
**7. Создать новый терминал и найти логи**
```

PS C:\WINDOWS\system32> minikube kubectl -- get pods
NAME                                   READY   STATUS    RESTARTS      AGE
deployment-react-768df6dc65-b5wgd      1/1     Running   0             15m
deployment-react-768df6dc65-pw2h6      1/1     Running   0             15m
frontend-deployment-5654bbf97f-4hmld   1/1     Running   3 (63m ago)   22d
frontend-deployment-5654bbf97f-kc5xf   1/1     Running   3 (63m ago)   22d
vault                                  1/1     Running   1 (63m ago)   77m
PS C:\WINDOWS\system32> minikube kubectl -- logs deployment-react-768df6dc65-b5wgd
Builing frontend
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
build finished
Server started on port 3000
PS C:\WINDOWS\system32> minikube kubectl -- logs deployment-react-768df6dc65-pw2h6
Builing frontend
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
Browserslist: caniuse-lite is outdated. Please run:
  npx update-browserslist-db@latest
  Why you should do it regularly: https://github.com/browserslist/update-db#readme
build finished
Server started on port 3000
```

