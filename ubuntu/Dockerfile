FROM ubuntu:16.04

LABEL Name="bourgeoisrebel/azadmin"

ADD ubuntu/Zscaler_Root_CA.crt /usr/local/share/ca-certificates/Zscaler_Root_CA.crt
RUN chmod 644 /usr/local/share/ca-certificates/Zscaler_Root_CA.crt && \
  apt-get update && apt-get install -y ca-certificates  &&\
  update-ca-certificates

# ADD ubuntu/zscaler.pem /usr/local/share/ca-certificates/zscaler.pem
# RUN chmod 644 /usr/local/share/ca-certificates/zscaler.pem && \
#   apt-get update && apt-get install -y ca-certificates  &&\
#   update-ca-certificates

RUN cd /tmp && \
    apt update && \
    apt-get install -y wget unzip curl lsb-release gnupg python3-pip python3-minimal && \
    apt-get install -y git 

# Install Azure CLI
RUN apt-get update && apt-get install -y apt-transport-https ca-certificates  && \
    curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
    gpg --dearmor | \
    tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null && \
    AZ_REPO=$(lsb_release -cs) && \
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    tee /etc/apt/sources.list.d/azure-cli.list && \
    apt-get update && \
    apt-get install -y azure-cli && \
    az extension add --name azure-devops

ADD ubuntu/zscaler.pem /tmp/zscaler.pem
RUN chmod 644 /tmp/zscaler.pem && cat /tmp/zscaler.pem >> /opt/az/lib/python3.6/site-packages/certifi/cacert.pem
    
RUN wget https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip --no-check-certificate && \
    unzip ./terraform_0.12.24_linux_amd64.zip -d /usr/local/bin/

# Install powershell
# RUN for apt_file in `grep -lr microsoft /etc/apt/sources.list.d/`; do rm $apt_file; done && \
#     apt-get update && apt-get install -y apt-transport-https && \
#     curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
#     sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-jessie-prod jessie main" > /etc/apt/sources.list.d/microsoft.list'  && \
#     apt-get update && \
#     apt-get install -y powershell
RUN wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb --no-check-certificate  && \
    dpkg -i packages-microsoft-prod.deb && \
    apt-get update && apt-get install -y apt-transport-https ca-certificates  && \
    apt-get install -y powershell && \
    pwsh -command "Install-Module -Name Az -AllowClobber -Force -Confirm:\$false && Import-Module -Name Az"

# Install helm/tiller
RUN HELM_VERSION=v2.16.1 && \
    wget https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz --no-check-certificate && \
    helm_sha256=7eebaaa2da4734242bbcdced62cc32ba8c7164a18792c8acdf16c77abffce202 && \
    echo "$helm_sha256 helm-${HELM_VERSION}-linux-amd64.tar.gz" | sha256sum -c - && \
    tar xzvf helm-${HELM_VERSION}-linux-amd64.tar.gz && \
    mv linux-amd64/helm /usr/local/bin && mv linux-amd64/tiller /usr/local/bin && \
    rm -rf linux-amd64 helm-${HELM_VERSION}-linux-amd64.tar.gz && \
    helm init --client-only

# Install kubectl
RUN apt-get update && apt-get install -y apt-transport-https && \
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list && \
    apt-get update && \
    apt-get install -y kubectl
    
# Install Hugo framework    
RUN apt-get update && apt-get install -y hugo
CMD [ "bash" ]
