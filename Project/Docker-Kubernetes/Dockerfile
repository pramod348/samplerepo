FROM redhat/ubi9:latest
MAINTAINER demo@training.com
ENV tomcat_test 123
RUN yum update -y
RUN mkdir -p /opt/tomcat/
WORKDIR /opt/tomcat
RUN curl -O http://archive.apache.org/dist/tomcat/tomcat-8/v8.5.40/bin/apache-tomcat-8.5.40.tar.gz
RUN tar xvfz apache*.tar.gz
RUN mv apache-tomcat-8.5.40/* /opt/tomcat/
RUN yum -y install java-1.8.0*
RUN java -version
WORKDIR /opt/tomcat/webapps
COPY  ./Project/Docker-Kubernetes/webapp.war /opt/tomcat/webapps/
EXPOSE 8080
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
