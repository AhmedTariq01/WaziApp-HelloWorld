FROM python:slim-buster

COPY . /root/src/

RUN  apt-get update \
     && apt-get install -y \
     curl \
     zip \
     && cd /root/src/ \
     && zip /index.zip docker-compose.yml package.json

#----------------------------#

# Uncomment For development
#ENTRYPOINT ["tail", "-f", "/dev/null"]


# Uncomment For production
ENTRYPOINT ["python", "/root/src/main.py"]

# Avoid warnings by switching to noninteractive
ENV POETRY_VERSION=1.0.0 \
    CLOUD_SDK_VERSION=277.0.0 \
    TERRAFORM_VERSION=0.11.14 \
    HELM_VERSION=3.2.0 \
    PYTHONFAULTHANDLER=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONHASHSEED=random \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    DEBIAN_FRONTEND=noninteractive

# gcloud sdk 
ENV PATH /google-cloud-sdk/bin:$PATH
RUN curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    tar xzf google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    rm google-cloud-sdk-${CLOUD_SDK_VERSION}-linux-x86_64.tar.gz && \
    ln -s /lib /lib64 && \
    gcloud config set core/disable_usage_reporting true && \
    gcloud config set component_manager/disable_update_check false && \
    gcloud components install kubectl && \
    gcloud components install beta
# Here is how you can access inside your container:
# sudo docker exec -it waziup.hello-world-python sh