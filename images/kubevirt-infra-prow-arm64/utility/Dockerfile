FROM prow_builder:latest as builder

ARG POD_UTILITY

FROM alpine:latest
ARG POD_UTILITY
ENV EP_CMD=$POD_UTILITY
RUN apk add --no-cache git openssh ca-certificates && update-ca-certificates

COPY --from=builder /prow/${POD_UTILITY} /${POD_UTILITY}

ENTRYPOINT /${EP_CMD}
