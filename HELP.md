## Infrastructure configuration

---

### 1. Clonar os demais repositórios para cá. O resultado será:

+ myteststore
  - angular-fe
  - api-gateway
  - k8s
  - logistic
  - order
  - store
  - config.json
  - docker-compose.yml
  - docker_postgres_init.sql

### 2. Edit line 48 to set the correct path to this folder.

``` yaml
volumes:
      - /home/fmm/Documents/myteststore:/usr/share/nginx/html/assets
```

### 3. Run the following command to get the environment working in Docker:

```
docker network create myteststore-network

docker-compose up
```

---

### 4. To generate images for Minikube, run the following commands (inside the correct folder):

```
docker build -t myteststore/web:3.0.0 .
docker build -t myteststore/gateway:3.0.0 .
docker build -t myteststore/store:3.0.0 .
docker build -t myteststore/order:3.0.0 .
docker build -t myteststore/logistic:3.0.0 .
```

### 5. Create the namespace:

```
kubectl create namespace myteststore
```

### 6. Add images to Minikube cache:

```
minikube cache add myteststore/web:3.0.0
minikube cache add myteststore/gateway:3.0.0
minikube cache add myteststore/store:3.0.0
minikube cache add myteststore/order:3.0.0
minikube cache add myteststore/logistic:3.0.0
```

### 7. Configure the application by applying the files from the K8s folder (as shown in the example below):

```
kubectl apply -f pv-volume.yml
kubectl apply -f pv-claim.yml
kubectl apply -f postgres-secrets.yml
kubectl apply -f postgres-configmap.yml
kubectl apply -f postgres-deployment.yml
kubectl apply -f postgres-service.yml
...
```
