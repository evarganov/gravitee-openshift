#-------------------------------------------------------------------------------
# Copyright (C) 2015 The Gravitee team (http://gravitee.io)
#    Licensed under the Apache License, Version 2.0 (the "License");
#    you may not use this file except in compliance with the License.
#    You may obtain a copy of the License at
#            http://www.apache.org/licenses/LICENSE-2.0
#    Unless required by applicable law or agreed to in writing, software
#    distributed under the License is distributed on an "AS IS" BASIS,
#    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#    See the License for the specific language governing permissions and
#    limitations under the License.
#-------------------------------------------------------------------------------
FROM docker-registry.x5.ru/nginx:1.10.2-alpine
MAINTAINER Gravitee Team <http://gravitee.io>

ARG GRAVITEEIO_VERSION=1.25.5

COPY files/*.sh /usr/local/bin/
RUN chmod u+x /usr/local/bin/*.sh

# Update to get support for Zip/Unzip, Bash
RUN apk --update add zip unzip bash wget

ENV WWW_TARGET /var/www/html/
COPY nginx/default.conf /etc/nginx/conf.d/default.conf

RUN wget https://download.gravitee.io/graviteeio-apim/distributions/graviteeio-full-${GRAVITEEIO_VERSION}.zip --no-check-certificate -P /tmp/ \
    && unzip /tmp/graviteeio-full-${GRAVITEEIO_VERSION}.zip -d /tmp/ \
    && mkdir -p ${WWW_TARGET} \
    && mv /tmp/graviteeio-full-${GRAVITEEIO_VERSION}/graviteeio-management-ui*/* ${WWW_TARGET} \
    && rm -rf /tmp/*


ENV MGMT_API_URL http://managementapi-gravitee.192.168.99.101.nip.io/management/

RUN cp /var/www/html/constants.json /var/www/html/constants.json.template
COPY html/constants.json /var/www/html/constants.json

# forward request and error logs to docker log collector
#RUN ln -sf /dev/stdout /var/log/nginx/access.log \
#	&& ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
