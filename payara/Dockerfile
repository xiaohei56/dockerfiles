FROM openjdk:8
LABEL maintainer="Philip Riecks"

ENV PAYARA_ARCHIVE payara5
ENV DOMAIN_NAME domain1
ENV INSTALL_DIR /opt
RUN useradd -b /opt -m -s /bin/sh -d ${INSTALL_DIR} serveradmin && echo serveradmin:serveradmin | chpasswd

RUN curl -o ${INSTALL_DIR}/${PAYARA_ARCHIVE}.zip -L https://s3-eu-west-1.amazonaws.com/payara.fish/Payara+Downloads/5.184/payara-5.184.zip \ 
    && unzip ${INSTALL_DIR}/${PAYARA_ARCHIVE}.zip -d ${INSTALL_DIR} \ 
    && rm ${INSTALL_DIR}/${PAYARA_ARCHIVE}.zip \
    && chown -R serveradmin:serveradmin /opt \
    && chmod -R a+rw /opt

ENV PAYARA_HOME ${INSTALL_DIR}/${PAYARA_ARCHIVE}/glassfish
ENV DEPLOYMENT_DIR ${INSTALL_DIR}/deploy
RUN mkdir ${DEPLOYMENT_DIR}

USER serveradmin
EXPOSE 4848 8009 8080 8181

ENTRYPOINT ${PAYARA_HOME}/generate_deploy_commands.sh && ${PAYARA_HOME}/bin/asadmin start-domain -v --postbootcommandfile ${DEPLOY_COMMANDS} ${DOMAIN_NAME}
