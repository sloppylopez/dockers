FROM jenkins:latest


#http://updates.jenkins-ci.org/download/plugins/docker-build-step/

# Dependencies
RUN sudo apt-get update -qq && \
    apt-get install -y -qq curl \

RUN curl -sL http://updates.jenkins-ci.org/download/plugins/docker-build-step/ | sudo cp $1 $JENKINS_HOME/plugins

# for main web interface:
EXPOSE 8080

# will be used by attached slave agents:
EXPOSE 50000