FROM ubuntu:latest
MAINTAINER blacktop, https://github.com/blacktop

#Prevent daemon start during install
RUN echo '#!/bin/sh\nexit 101' > /usr/sbin/policy-rc.d && \
    chmod +x /usr/sbin/policy-rc.d

# Install Requirements
RUN apt-get update -qq && apt-get install -qqy libc6-i386 supervisor wget

# Add EICAR Test Virus File to malware folder
ADD http://www.eicar.org/download/eicar.com.txt /malware/EICAR

# Get Avast Installer
ADD http://files.avast.com/files/linux/avast4workstation_1.3.0-2_i386.deb /avast/

# Install Avast
RUN dpkg -i /avast/avast4workstation_1.3.0-2_i386.deb

# Start Avast Service and Update Avast Definitions
ADD avastrc .avast/avastrc

RUN sysctl -w kernel.shmmax=128000000
RUN /usr/bin/avast && /usr/bin/avast-update

# Try to reduce size of container.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /malware

ENTRYPOINT ["/usr/bin/avast"]

CMD ["--help"]
