# The FROM will be replaced when building in OpenShift
FROM centos:7

MAINTAINER Siamak Sadeghianfar <ssadeghi@redhat.com>

# Install headless Java
USER root
ENV GO_VERSION_DEFAULT=1.12.1 \
    GOROOT=/usr/local/go \
    GOPATH=/usr/src/go
ENV PATH=$GOPATH/bin:$GOROOT/bin:$PATH

WORKDIR /opt

RUN yum-config-manager --disable rhel* 1>/dev/null && \
    yum-config-manager --enable rhel-7-server-rpms && \
    yum-config-manager --enable rhel-7-server-ose-3.2-rpms && \
    yum-config-manager --enable rhel-7-optional-rpms && \
    yum-config-manager --enable rhel-7-extras-rpms && \
    yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum clean all && \
    export INSTALL_PKGS="nss_wrapper java-1.8.0-openjdk-headless \
        java-1.8.0-openjdk-devel nss_wrapper gettext tar git curl gcc \
        unzip file wget" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    yum clean all && \
    mkdir -p /var/lib/jenkins && \
    chown -R 1001:0 /var/lib/jenkins && \
    chmod -R g+w /var/lib/jenkins

RUN if [ -z $GO_VERSION ] ; then GO_VERSION=${GO_VERSION_DEFAULT} ; fi && \
    curl -L -o /usr/go${GO_VERSION}.linux-amd64.tar.gz https://dl.google.com/go/go${GO_VERSION}.linux-amd64.tar.gz && \
    mkdir -p /usr/src/go/src/redhat && \
    tar -xzf /usr/go${GO_VERSION}.linux-amd64.tar.gz && \
    mv $(pwd)/go /usr/local/ && \
    go get -u github.com/golang/dep/cmd/dep && \
    chown -R 1001 /usr/src/go && \
    chown -R 1001 /usr/local/go && \
    chown -R 1001 ${HOME}/.cache/go-build && \
    rm -f /usr/go${GO_VERSION}.linux-amd64.tar.gz

# Copy the entrypoint
COPY configuration/* /var/lib/jenkins/
USER 1001

# Run the JNLP client by default
# To use swarm client, specify "/var/lib/jenkins/run-swarm-client" as Command
ENTRYPOINT ["/var/lib/jenkins/run-jnlp-client"]
