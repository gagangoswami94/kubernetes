## PART-1    Labeling Deployment
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: test-1
    env: dev
```
## PART-2    Setting Replica count to 2 & setting selectors to match pods to add in deployment from PART-3
```
spec:
  replicas: 2
  selector:
    matchLabels:
      name: nginx-dev-pods
```

## PART-3    Adding labels to pods
```
  template:
    metadata:
      labels:
        name: nginx-dev-pods

```
## PART-4    Defining Container
```
    spec:
      containers:
      - image: nginx:1.14.2
        name: nginx
        ports:
        - containerPort: 80
          name:  http-port
```
