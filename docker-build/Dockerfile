# Get the latest Pi-hole version
FROM pihole/pihole:2022.01.1

# Install necessary programs
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
    unbound \
    wget

# Additional configs
# unbound
COPY unbound-v1.9_pihole.conf  /etc/unbound/unbound.conf.d/pihole.conf

# Download the DNS-Root-Zone. Change owner and move it to the right directory
RUN wget -O root.hints https://www.internic.net/domain/named.root && \
    chown unbound:unbound root.hints && \
    mv root.hints /var/lib/unbound/


# Install pihole-updatelists
RUN apt-get update && apt-get install -Vy php-cli php-sqlite3 php-intl php-curl
RUN wget -O - https://raw.githubusercontent.com/jacklul/pihole-updatelists/master/install.sh | bash /dev/stdin docker

# Unbound startup script
COPY start_unbound_and_s6_init.sh start_unbound_and_s6_init.sh
RUN chmod +x start_unbound_and_s6_init.sh
ENTRYPOINT ./start_unbound_and_s6_init.sh
