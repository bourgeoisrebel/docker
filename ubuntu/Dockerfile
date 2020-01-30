FROM ubuntu

LABEL Name="bourgeoisrebel/azadmin"

RUN cd /tmp && \
    apt update && \
    apt-get install -y wget unzip curl lsb-release gnupg python3-pip python3-minimal

RUN curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
    gpg --dearmor | \
    tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null && \
    AZ_REPO=$(lsb_release -cs) && \
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    tee /etc/apt/sources.list.d/azure-cli.list && \
    apt-get update && \
    apt-get install -y azure-cli && \
    az extension add --name azure-devops
    
RUN wget https://releases.hashicorp.com/terraform/0.12.12/terraform_0.12.12_linux_amd64.zip --no-check-certificate && \
    unzip ./terraform_0.12.12_linux_amd64.zip -d /usr/local/bin/

RUN wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb --no-check-certificate && \
    dpkg -i packages-microsoft-prod.deb && \
    apt update && \
    apt install -y powershell && \
    pwsh -Command "Install-Module -Name Az -AllowClobber -Force"
    
RUN apt-get update && \
    apt-get install -y apt-transport-https \
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - \
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list \
    apt-get update \
    apt-get install -y kubectl
    
RUN curl -LO https://get.helm.sh/helm-v2.14.2-linux-amd64.tar.gz
    tar zxvf ./helm-v2.14.2-linux-amd64.tar.gz
    mv linux-amd64/helm . && chmod +x ./helm
    rm -rf ./helm-v2.14.2-linux-amd64.tar.gz
    rm -rf ./linux-amd64

CMD [ "bash" ]
