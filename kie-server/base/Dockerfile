#################################################################################
# Dockerfile that provides the image for JBoss KIE Server 7.18.0.Final
#################################################################################

####### BASE ############
FROM jboss/wildfly:14.0.1.Final

####### MAINTAINER ############
MAINTAINER "PatSim"

####### ENVIRONMENT ############
ENV JBOSS_BIND_ADDRESS 0.0.0.0
ENV KIE_REPOSITORY https://repository.jboss.org/nexus/content/groups/public-jboss
ENV KIE_VERSION 7.18.0.Final
ENV KIE_CLASSIFIER ee7
ENV KIE_CONTEXT_PATH kie-server
ENV JAVA_OPTS -Xms256m -Xmx1024m -Djava.net.preferIPv4Stack=true -Dfile.encoding=UTF-8

###### SWITCH USER root ######
USER 1001

###### INSTALL PACKAGES ######
RUN yum install -y net-tools

##### SWITCH BACK TO jboss ####
# USER 1002
####### KIE-SERVER ############
RUN curl -o $HOME/$KIE_CONTEXT_PATH.war $KIE_REPOSITORY/org/kie/server/kie-server/$KIE_VERSION/kie-server-$KIE_VERSION-$KIE_CLASSIFIER.war && \
unzip -q $HOME/$KIE_CONTEXT_PATH.war -d $JBOSS_HOME/standalone/deployments/$KIE_CONTEXT_PATH.war &&  \
touch $JBOSS_HOME/standalone/deployments/$KIE_CONTEXT_PATH.war.dodeploy &&  \
rm -rf $HOME/$KIE_CONTEXT_PATH.war

####### SCRIPTS ############
USER 1001
ADD etc/start_kie-wb.sh $JBOSS_HOME/bin/start_kie-wb.sh
RUN chown jboss:jboss $JBOSS_HOME/bin/start_kie-wb.sh

####### CUSTOM JBOSS USER ############
# Switchback to jboss user
# USER 1002

####### RUNNING DROOLS-WB ############
WORKDIR $JBOSS_HOME/bin/
CMD ["./start_kie-wb.sh"]

USER 1001
ENTRYPOINT ["/tmp/docker_run.sh"]
