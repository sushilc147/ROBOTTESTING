FROM alpine:3.10.1

# PYTHON
RUN apk add --no-cache python3 py3-pip
RUN pip3 install --upgrade pip
RUN apk info -a zlib
#RUN apt install docker-compose -y
RUN apk update && \
    apk add --no-cache docker-cli python3 && \
    apk add --no-cache --virtual .docker-compose-deps python3-dev libffi-dev openssl-dev gcc libc-dev make && \
    pip3 install docker-compose && \
    apk del .docker-compose-deps
# CHROME
RUN apk --no-cache upgrade && apk add --no-cache chromium chromium-chromedriver
RUN apk add libexif udev

# ROBOT + LIBRARIES
COPY requirements.txt /tmp/requirements.txt
RUN  pip3 install -r /tmp/requirements.txt
