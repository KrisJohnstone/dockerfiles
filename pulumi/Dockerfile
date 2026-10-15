FROM golang:1.27-trixie

RUN apt-get update -y && \
    curl -fsSL https://get.pulumi.com | sh
    
ENV PATH="/root/.pulumi/bin:${PATH}"

ENTRYPOINT ["pulumi"]
