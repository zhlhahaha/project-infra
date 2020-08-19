FROM ubuntu:18.04

ARG CO_COMMIT=9e73c7faec632f528654225e2a7c31ce66890331

COPY bootstrap-arm64.patch /tmp/bootstrap-arm64.patch

RUN apt-get update && apt-get install -y git

RUN git clone https://github.com/kubernetes/test-infra.git \
    && cd test-infra \
    && git checkout ${CO_COMMIT} \
    && patch -p1 < /tmp/bootstrap-arm64.patch

RUN cd test-infra \
&& docker build images/bootstrap/ -t bootstrap-arm64:latest \
&& docker image list
