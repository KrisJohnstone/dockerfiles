FROM ice88/awscli:latest

RUN dnf update -y && \
    dnf install -y yum-utils shadow-utils && \
    yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo && \
    yum install packer -y

ENTRYPOINT ["packer"]
