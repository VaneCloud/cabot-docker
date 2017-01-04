# Cabot Dockerfile
#
# fork by https://github.com/shoonoise/cabot-docker
#
# VERSION 1.1

FROM ubuntu:trusty
MAINTAINER Alexander Kushnarev <avkushnarev@gmail.com>

# Prepare
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y build-essential nodejs libpq-dev python-dev npm git curl libldap2-dev libsasl2-dev iputils-ping

RUN curl -L 112.64.137.178/get-pip.py | python
RUN mkdir /root/.pip
ADD pip.conf /root/.pip/pip.conf

RUN mkdir /cabot
WORKDIR /cabot

ADD requirements.txt .
ADD cabot-app/cabot/requirements-plugins.txt .
# Install dependencies
RUN pip install -r requirements.txt
RUN pip install -r requirements-plugins.txt

RUN npm install --no-color -g coffee-script less@1.3 --registry=https://registry.npm.taobao.org

# Deploy cabot
#RUN git clone http://gitlab.vanecloud.com/vanecloud/cabot.git /cabot
ADD cabot-app/cabot /cabot/
ADD fixture.json /cabot/
ADD run.sh /cabot/
ADD gunicorn.conf /cabot/gunicorn.conf

# Set env var
ENV PATH $PATH:/cabot/
ENV PYTHONPATH $PYTHONPATH:/cabot/

# Cabot settings
ENV DJANGO_SETTINGS_MODULE cabot.settings
ENV HIPCHAT_URL https://api.hipchat.com/v1/rooms/message
ENV LOG_FILE /var/log/cabot
ENV PORT 5000
ENV ADMIN_EMAIL you@example.com
ENV CABOT_FROM_EMAIL noreply@example.com
ENV DEBUG t
ENV DB_HOST db
ENV DB_PORT 5432
ENV DB_USER docker
ENV DB_PASS docker

ENV DJANGO_SECRET_KEY 2FL6ORhHwr5eX34pP9mMugnIOd3jzVuT45f7w430Mt5PnEwbcJgma0q8zUXNZ68A

# Used for pointing links back in alerts etc.
ENV WWW_HTTP_HOST cabot.example.com
ENV WWW_SCHEME http

RUN ["ln","-s","/usr/bin/nodejs","/usr/bin/node"]

EXPOSE 5000

WORKDIR /cabot/
CMD . /cabot/run.sh
