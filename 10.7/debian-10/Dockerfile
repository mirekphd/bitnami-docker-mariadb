FROM docker.io/bitnami/minideb:buster
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libaio1 libaudit1 libc6 libcap-ng0 libgcc1 libicu63 liblzma5 libncurses6 libpam0g libssl1.1 libstdc++6 libtinfo6 libxml2 procps psmisc tar zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "ini-file" "1.4.1-7" --checksum 7a4f45cbebd2e242278060415dd1a3f0dbe68991fdcaea5bb311e8de54a3e027
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mariadb" "10.7.3-0" --checksum 34769719f807814ec1951b91082de6a7fbf2ed5eca1340a5049a80beb9a165d7
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-7" --checksum d6280b6f647a62bf6edc74dc8e526bfff63ddd8067dcb8540843f47203d9ccf1
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN mkdir /docker-entrypoint-initdb.d

COPY rootfs /
RUN /opt/bitnami/scripts/mariadb/postunpack.sh
ENV APP_VERSION="10.7.3" \
    BITNAMI_APP_NAME="mariadb" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/mariadb/bin:/opt/bitnami/mariadb/sbin:$PATH"

EXPOSE 3306

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/mariadb/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/mariadb/run.sh" ]
