# Create MLFlow service running on kubernetes

Follow along in order

Inspiration:

[2021 Article](https://medium.com/artefact-engineering-and-data-science/serving-ml-models-at-scale-using-mlflow-on-kubernetes-bf27258775e7)

[Minikube 2020 article](https://towardsdatascience.com/mlflow-part-2-deploying-a-tracking-server-to-minikube-a2d6671e6455)

## Step 1: Create Kind Cluster (If using Kind)
```
kind create cluster --name mlflow-cluster --config=kind-spec.yaml
```

## Step 2: Add Postgres Repo to Helm
```
#docs: https://artifacthub.io/packages/helm/bitnami/postgresql
helm repo add bitnami https://charts.bitnami.com/bitnami
```

## Step 3: Install Postgres using Helm

```
helm upgrade mlflow-postgres -f postgres/values.yaml bitnami/postgresql --install
```

## Step 4: Install Minio
```
helm upgrade -f minio/values.yaml mlflow-minio minio/minio --install
```

## (Optional) Step 5: Build mLFlow Container

If you want to build your own container of MLFlow you can use the dockerfile in the directory.  Otherwise the image is uploaded to dockerhub [DockerHub MLFlow Server](https://hub.docker.com/repository/docker/joshsgoldstein/mlflow-server)
```
docker build --tag joshsgoldstein/mlflow-server:latest ml-flow/.
```

## Step 6: Apply Deployment YAML for MLFlow Container
```
k apply -f ml-flow-deployment.yaml
```

