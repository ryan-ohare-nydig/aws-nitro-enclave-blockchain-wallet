#  Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
#  SPDX-License-Identifier: MIT-0
FROM amazonlinux:2

ARG HSM_IP
ARG CA_CLIENT_CERT_PATH

RUN yum install -y https://s3.amazonaws.com/cloudhsmv2-software/CloudHsmClient/EL7/cloudhsm-client-latest.el7.x86_64.rpm
RUN yum install gcc python3 python3-devel net-tools -y

WORKDIR /app

COPY ./kms/kmstool_enclave_cli ./
COPY ./kms/libnsm.so /usr/lib64/

COPY requirements.txt ./
RUN pip3 install -r /app/requirements.txt

ARG REGION_ARG=us-east-1
ENV REGION=$REGION_ARG

COPY server.py ./

#
# Setup the HSM Shit
#
COPY ${CA_CLIENT_CERT_PATH} /opt/cloudhsm/etc/
RUN /opt/cloudhsm/bin/configure -a ${HSM_IP}

CMD [ "python3", "/app/server.py" ]