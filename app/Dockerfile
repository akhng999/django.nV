FROM python:3.8.3

ENV APP_ROOT /
ENV CONFIG_ROOT /config


RUN mkdir ${CONFIG_ROOT}
COPY requirements.txt ${CONFIG_ROOT}/requirements.txt
RUN pip install -r ${CONFIG_ROOT}/requirements.txt

#RUN mkdir ${APP_ROOT}
WORKDIR ${APP_ROOT}

ADD . ${APP_ROOT}

COPY reset_db.sh runapp.sh /root/
RUN chmod +x reset_db.sh
RUN ./reset_db.sh