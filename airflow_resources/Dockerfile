FROM apache/airflow:2.7.3

# Install additional dependencies
RUN pip install --no-cache-dir kafka-python==2.0.2 unidecode==1.3.7

# Ensure the airflow user has the correct UID and GID
ARG AIRFLOW_UID=1000
ARG AIRFLOW_GID=1000
USER ${AIRFLOW_UID}:${AIRFLOW_GID}