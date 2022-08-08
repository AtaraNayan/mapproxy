#--------- Generic stuff all our Dockerfiles should start with so we get caching ------------
FROM python:3.9
MAINTAINER Tim Sutton<tim@kartoza.com>

#-------------Application Specific Stuff ----------------------------------------------------

RUN apt-get -y update && \
    apt-get install -y \
    gettext \
    python3-yaml \
    libgeos-dev \
    python3-lxml \
    libgdal-dev \
    build-essential \
    python-dev \
    libjpeg-dev \
    zlib1g-dev \
    libfreetype6-dev \
    python3-virtualenv \
    gosu awscli; \
    # verify that the binary works
    gosu nobody true
COPY /mapproxy-master.zip /mapproxy-master.zip

RUN pip3 --disable-pip-version-check install Shapely /mapproxy-master.zip Pillow uwsgi pyproj boto3 s3cmd \
    requests riak==2.4.2 redis

# RUN virtualenv --system-site-packages mapproxy

# RUN apt-get -y update
# RUN apt-get install unzip

RUN mkdir -p /mapproxy
COPY /mapproxy_configuration/* /mapproxy
# RUN pip install mapproxy-master.zip
# COPY /mymapproxy.zip /mymapproxy.zip

# mapproxy-master.zip
# Cleanup resources
RUN apt-get -y --purge autoremove  \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
EXPOSE 8080

ADD build_data/uwsgi.ini /settings/uwsgi.default.ini
ADD build_data/multi_mapproxy.py /multi_mapproxy.py
ADD scripts /scripts
RUN chmod +x /scripts/*.sh

ENTRYPOINT [ "/scripts/start.sh" ]
CMD [ "/scripts/run_develop_server.sh" ]
