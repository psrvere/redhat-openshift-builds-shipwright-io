FROM brew.registry.redhat.io/rh-osbs/openshift-golang-builder:rhel_9_1.23 AS builder

ENV GOEXPERIMENT=strictfipsruntime

COPY build .

RUN CGO_ENABLED=1 GO111MODULE=on go build -a -mod=vendor -ldflags="-s -w" -tags="strictfipsruntime" -o openshift-builds-controller-test ./cmd/shipwright-build-controller

FROM registry.access.redhat.com/ubi9-minimal@sha256:ac61c96b93894b9169221e87718733354dd3765dd4a62b275893c7ff0d876869

ENV PATH="$PATH:/ko-app"

COPY --from=builder /openshift-builds-controller-test .
COPY LICENSE /licenses/

USER 1000

ENTRYPOINT ["./openshift-builds-controller-test"]

LABEL \
    com.redhat.component="openshift-builds-controller-test" \
    name="openshift-builds-test/controller" \
    version="v1.4-test" \
    summary="Red Hat OpenShift Builds Controller Test" \
    maintainer="openshift-builds@redhat.com" \
    description="Red Hat OpenShift Builds Controller Test" \
    io.k8s.description="Red Hat OpenShift Builds Controller Test" \
    io.k8s.display-name="Red Hat OpenShift Builds Controller Test" \
    io.openshift.tags="builds-test,controller-test"