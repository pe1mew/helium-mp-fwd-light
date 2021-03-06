ARG SYSTEM_TIMEZONE="Europe/London"

FROM balenalib/raspberry-pi-debian:buster-run

# Move to working directory
WORKDIR /opt/nebra-gatewayrs

# Intall dependencies
RUN \
    apt-get update && \
    DEBIAN_FRONTEND="noninteractive" \
    TZ="$SYSTEM_TIMEZONE" \
        apt-get -y install \
        wget=1.20.1-1.1 \
        python3=3.7.3-1 \
        ca-certificates=20200601~deb10u2 \
        --no-install-recommends && \
        apt-get autoremove -y && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

# Pull in latest helium gatewayrs deb file and install
# Update to meet latest release as available at 13-10-2021
RUN wget https://github.com/helium/gateway-rs/releases/download/v1.0.0-alpha.19/helium-gateway-v1.0.0-alpha.19-raspi234.deb
RUN dpkg -i helium-gateway-*-raspi234.deb

# Copy start script and settings file
COPY start-gatewayrs.sh .
COPY keys.py .
COPY settings.toml.template /etc/helium_gateway/settings.toml.template

# Run start-gatewayrs script
ENTRYPOINT ["/opt/nebra-gatewayrs/start-gatewayrs.sh"]
