FROM centos:latest
LABEL Name="bourgeoisrebel/azadmin-centos"

COPY /cert.pem /etc/pki/ca-trust/source/anchors
RUN update-ca-trust force-enable && update-ca-trust extract

RUN yum -y install \
        epel-release \
        make \
        gcc \
        git \
        openssl-devel \
        libxml2-devel

WORKDIR /tmp

RUN yum -y install https://centos7.iuscommunity.org/ius-release.rpm
RUN yum -y install \
        python36u \
        python36u-libs \
        python36u-devel \
        python36u-pip

RUN yum -y install https://github.com/PowerShell/PowerShell/releases/download/v6.2.0/powershell-6.2.0-1.rhel.7.x86_64.rpm
RUN pwsh -Command Install-Module -Name Az -force

RUN rpm --import https://packages.microsoft.com/keys/microsoft.asc
RUN sh -c 'echo -e "[azure-cli]\nname=Azure CLI\nbaseurl=https://packages.microsoft.com/yumrepos/azure-cli\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'
RUN yum -y install azure-cli --nogpgcheck

RUN yum -y install \
        wget \
        unzip \
        mlocate 

RUN az extension add --name azure-devops

RUN wget https://releases.hashicorp.com/terraform/0.11.14/terraform_0.11.14_linux_amd64.zip
RUN unzip terraform_0.11.14_linux_amd64.zip
RUN rm -rf terraform_0.11.14_linux_amd64.zip
RUN mv terraform* /usr/bin/terraform 

RUN pip3.6 list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip3.6 install -U
RUN pip3.6 install click azure-devops jinja2
RUN curl -fsSL https://get.pulumi.com | sh

ENV LC_ALL en_US.utf-8
ENV LANG en_US.utf-8

RUN yum clean all

CMD [ "bash" ]
