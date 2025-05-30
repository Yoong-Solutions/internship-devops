FROM centos:7

RUN yum install -y unzip curl iproute && \
    curl -LO https://oss.chaosblade.io/bin/chaosblade-1.9.0-linux-amd64.zip && \
    unzip chaosblade-1.9.0-linux-amd64.zip && \
    mv chaosblade-1.9.0 /opt/chaosblade && \
    ln -s /opt/chaosblade/blade /usr/local/bin/blade && \
    chmod +x /opt/chaosblade/blade

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

CMD ["/entrypoint.sh"]
