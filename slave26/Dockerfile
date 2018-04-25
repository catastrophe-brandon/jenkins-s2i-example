# The FROM will be replaced when building in OpenShift
FROM rhel6/rhel

MAINTAINER Siamak Sadeghianfar <ssadeghi@redhat.com>

# Install headless Java
USER root
RUN subscription-manager repos --disable rhel* 1>/dev/null && \
    subscription-manager repos --enable rhel-6-server-rpms && \
    subscription-manager repos --enable rhel-6-server-ose-3.2-rpms && \
    yum clean all && \
    export INSTALL_PKGS="nss_wrapper java-1.8.0-openjdk-headless \
        java-1.8.0-openjdk-devel nss_wrapper gettext tar git python curl gcc python-devel \
        unzip file" && \
    yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    yum clean all && \
    mkdir -p /var/lib/jenkins && \
    chown -R 1001:0 /var/lib/jenkins && \
    chmod -R g+w /var/lib/jenkins && \
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python get-pip.py && \
    pip install virtualenv 

# Copy the entrypoint
COPY configuration/* /var/lib/jenkins/
USER 1001

# Run the JNLP client by default
# To use swarm client, specify "/var/lib/jenkins/run-swarm-client" as Command
ENTRYPOINT ["/var/lib/jenkins/run-jnlp-client"]
