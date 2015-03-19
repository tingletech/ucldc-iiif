FROM ubuntu:14.04

MAINTAINER Barbara Hui <barbara.hui@ucop.edu>

RUN apt-get update && \
  apt-get install -y \
    apache2 \
    curl \
    git \
    libapache2-mod-wsgi \
    libfreetype6 \
    libfreetype6-dev \
    libjpeg-turbo8 \ 
    libjpeg-turbo8-dev \
    liblcms2-2 \
    liblcms2-dev \
    liblcms-utils \
    libtiff5-dev \
    libwebp-dev \
    python-dev \
    python-pip \
    python-setuptools \
    zlib1g-dev && \
  pip install \
    boto \
    configobj \
    mock \
    Pillow \
    requests \
    responses \
    werkzeug && \
  # have to do this to get rid of "ImportError: No module named _winreg" error for six.py
  pip install --upgrade six && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ENV PATH $PATH:/opt/loris/bin/Linux/x86_64
ENV LD_LIBRARY_PATH /opt/loris/lib/Linux/x86_64

RUN useradd -d /var/www/loris2 -s /sbin/false loris

# make image directories
RUN mkdir /usr/local/share/images
RUN mkdir /usr/local/share/images/loris
RUN chown loris:loris /usr/local/share/images
RUN chown loris:loris /usr/local/share/images/loris

# install Loris
WORKDIR /opt
RUN curl -s -L https://github.com/barbarahui/loris/archive/ucldc.tar.gz \
  | tar zxf - --transform s/^loris-ucldc/loris/ && \
  python /opt/loris/setup.py install && \
  rm -rf /opt/loris/docker/ /opt/loris/tests/

# set up apache
RUN a2enmod headers expires
COPY apache-config.txt /etc/apache2/sites-enabled/000-default.conf

# cache maintenance - need to set up a cron job

EXPOSE 80 

CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
