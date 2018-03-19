FROM ubuntu
MAINTAINER alisson.machado@4linux.com.br

RUN apt clean all
RUN apt update

RUN apt install git python python-pip s3fs -y
RUN rm -rf /srv/python-openshift/
RUN git clone -b sessao https://github.com/AlissonMMenezes/python-openshift.git /srv/python-openshift/
RUN pip install -r /srv/python-openshift/requirements.txt
RUN sed -i "s/minishift/docker/g" /srv/python-openshift/app.py


ENTRYPOINT ["python","/srv/python-openshift/app.py"]
