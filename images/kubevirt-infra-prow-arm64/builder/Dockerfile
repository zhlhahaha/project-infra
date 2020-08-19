FROM ubuntu:18.04 as builder

ARG BAZEL_VERSION=3.2.0
ARG CO_COMMIT=master

COPY nodejs.patch /tmp

RUN apt-get update && apt-get install -y \
    default-jdk \
    g++ \
    git \
    python \
    python3 \
    python3-dev \
    build-essential \
    zip \
    unzip \
    wget 

RUN mkdir bazel \
    && cd bazel \
    && wget https://github.com/bazelbuild/bazel/releases/download/${BAZEL_VERSION}/bazel-${BAZEL_VERSION}-dist.zip \
    && unzip bazel-${BAZEL_VERSION}-dist.zip \
    && bash ./compile.sh \
    && cp output/bazel /usr/local/bin/ \
    && cd ../ 

RUN git clone https://github.com/kubernetes/test-infra.git \
    && cd test-infra \
    && git checkout ${CO_COMMIT} \
    && patch -p1 < /tmp/nodejs.patch 

RUN cd test-infra \
&& bazel build //prow/cmd/clonerefs \
&& bazel build //prow/cmd/initupload \
&& bazel build //prow/cmd/entrypoint \
&& bazel build //prow/cmd/sidecar

RUN cd / && mkdir prow \
    && cp /test-infra/bazel-bin/prow/cmd/clonerefs/linux_arm64_pure_stripped/clonerefs /prow/ \
    && cp /test-infra/bazel-bin/prow/cmd/initupload/linux_arm64_pure_stripped/initupload /prow/ \
    && cp /test-infra/bazel-bin/prow/cmd/entrypoint/linux_arm64_pure_stripped/entrypoint /prow/ \
    && cp /test-infra/bazel-bin/prow/cmd/sidecar/linux_arm64_pure_stripped/sidecar /prow/

FROM ubuntu:18.04
RUN mkdir /prow/
COPY --from=builder /prow/* /prow/
