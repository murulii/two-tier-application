```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1  # Adjust the number of replicas as needed
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:latest  # Use the desired MySQL image
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: root
        - name: MYSQL_DATABASE
          value: default_db
        - name: MYSQL_USER
          value: admin
        - name: MYSQL_PASSWORD
          value: root
        ports:
        - containerPort: 3306
```
