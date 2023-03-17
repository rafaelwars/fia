FROM apache/airflow:2.3.3

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
           vim \
           telnet \
           curl \
           iputils-ping \
           systemctl \
           default-jdk \
           ca-certificates \
    && apt-get autoremove -yqq --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir /usr/local/airflow

ARG AIRFLOW_USER_HOME=/usr/local/airflow
ENV AIRFLOW_HOME=${AIRFLOW_USER_HOME}

RUN chown -R airflow: ${AIRFLOW_HOME}

USER airflow

RUN python -m pip install --upgrade pip

ENV FLASK_APP="airflow.www.app flask fab create-admin"

COPY ./entrypoint.sh /entrypoint.sh
COPY ./airflow.cfg ${AIRFLOW_HOME}/airflow.cfg
COPY ./connections.json ${AIRFLOW_HOME}/connections.json

EXPOSE 8080

WORKDIR ${AIRFLOW_HOME}

ENTRYPOINT ["/entrypoint.sh"]
