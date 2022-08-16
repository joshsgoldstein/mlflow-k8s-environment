Create mlFlow Tracking Container

Inspiration:

[2021 Article](https://medium.com/artefact-engineering-and-data-science/serving-ml-models-at-scale-using-mlflow-on-kubernetes-bf27258775e7)

[Minikube 2020 article](https://towardsdatascience.com/mlflow-part-2-deploying-a-tracking-server-to-minikube-a2d6671e6455)

Add Postgres to k8s cluster:
```
#docs: https://artifacthub.io/packages/helm/bitnami/postgresql
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install mlflow-postgres bitnami/postgresql --set postgresqlDatabase=mlflow_db --set postgresqlPassword=mlflow --set service.type=NodePort
```

Add Minio to the Cluster:
```
helm install mlflow-minio --set rootUser=minio,rootPassword=minio minio/minio
```

Build mLFlow Container

Deploy Container to k8s
```
helm install mlf-ts mlflow-tracking/mlflow-tracking-server \
--set env.mlflowArtifactPath=${GS_ARTIFACT_PATH} \
--set env.mlflowDBAddr=mlf-db-postgresql \
--set env.mlflowUser=postgres \
--set env.mlflowPass=mlflow \
--set env.mlflowDBName=mlflow_db \
--set env.mlflowDBPort=5432 \
--set service.type=LoadBalancer \
--set image.repository=test/mlflow-tracking \
--set image.tag=latest
```



Setup MySQL 