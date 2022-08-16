# Defining base image
FROM python:3.8.2-slim
# Installing packages from PyPi
RUN pip install mlflow[extras] && \
    pip install psycopg2-binary && \
    pip install boto3
# Defining start up command
EXPOSE 5000
ENTRYPOINT ["mlflow", "server"]