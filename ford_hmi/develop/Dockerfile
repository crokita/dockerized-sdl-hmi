# ##################################################################################################
# Basic docker build of SDL_Core. Totally not stolen from https://github.com/smartdevicelink/sdl_core
# ##################################################################################################

# This image is based of debian with curl preinstalled
FROM buildpack-deps:jessie-curl

MAINTAINER Chris Rokita version:0.1

ENV SDL_HMI_BRANCH develop
ENV SDL_CORE_NETWORK_INTERFACE eth0
ENV HMI_WEBSOCKET_ADDRESS 127.0.0.1:8087

# Download SDL HMI from github 
RUN mkdir /var/www \
    && curl -sSL https://github.com/smartdevicelink/sdl_hmi/archive/$SDL_HMI_BRANCH.tar.gz \
    | tar -v -C /var/www -xz --strip-components 1

#install jq to make it easy to parse JSON output
RUN curl http://stedolan.github.io/jq/download/linux64/jq -o /usr/sbin/jq \
	&& chmod a+x /usr/sbin/jq

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    nginx

# Delete default nginx configuration
RUN rm -v /etc/nginx/nginx.conf

# Copy our new configuration
COPY nginx.conf /etc/nginx/

# Copy SDL Core start script into container
COPY setup.sh setup.sh

# Configure logger
RUN ldconfig

# Expose the following ports
#   8080:  Returns index.html of HMI Core
EXPOSE 8080

# Run nginx
CMD ["/bin/bash", "setup.sh"]