    FROM openjdk:11 As BUILD_IMAGE
    RUN  apt update && apt install maven  -y 
    RUN  apt install -y  git 

    RUN git clone https://github.com/devopshydclub/vprofile-project.git && \
    cd vprofile-project && git checkout docker && mvn install

    FROM tomcat:9-jdk11
    RUN  rm -rf /usr/local/tomcat/webapps/*
    COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war  /usr/local/tomcat/webapps/ROOT.war 

    EXPOSE 8080
    CMD [ "catalina.sh","run" ] 
