# glassfish

#Dockerize glassfish
```
FROM        java:8-jdk

ENV         JAVA_HOME         /usr/lib/jvm/java-8-openjdk-amd64
ENV         GLASSFISH_HOME    /usr/local/glassfish5
ENV         PATH              $PATH:$JAVA_HOME/bin:$GLASSFISH_HOME/bin

RUN echo "deb [check-valid-until=no] http://archive.debian.org/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list

# As suggested by a user, for some people this line works instead of the first one. Use whichever works for your case
# RUN echo "deb [check-valid-until=no] http://archive.debian.org/debian jessie main" > /etc/apt/sources.list.d/jessie.list

RUN echo 'Acquire::Check-Valid-Until "false";' >> /etc/apt/apt.conf
RUN sed -i '/deb http:\/\/deb.debian.org\/debian jessie-updates main/d' /etc/apt/sources.list

#RUN apt-get -o Acquire::Check-Valid-Until=false update

RUN         apt-get update && \
            apt-get install -y curl unzip zip inotify-tools && \
            rm -rf /var/lib/apt/lists/*

RUN         curl -L -o /tmp/glassfish-5.0.zip http://download.java.net/glassfish/5.0/release/glassfish-5.0.zip && \
            unzip /tmp/glassfish-5.0.zip -d /usr/local && \
            rm -f /tmp/glassfish-5.0.zip

EXPOSE      8080 4848 8181

WORKDIR     /usr/local/glassfish5

# verbose causes the process to remain in the foreground so that docker can track it
CMD         asadmin start-domain --verbose
#CMD         asadmin deploy /path1/path2/myapp.war
```
