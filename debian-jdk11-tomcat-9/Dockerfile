FROM msjkus/debian-jdk11:latest

MAINTAINER "msjkus"
LABEL org.opencontainers.image.authors="msjkus"

ENV TOMCAT_MAJOR_VERSION 9
ENV TOMCAT_MINOR_VERSION 0
ENV TOMCAT_BUILD_VERSION 50
ENV TOMCAT_VERSION "${TOMCAT_MAJOR_VERSION}.${TOMCAT_MINOR_VERSION}.${TOMCAT_BUILD_VERSION}"

RUN	echo "downloading tomcat ${TOMCAT_VERSION} ..." && \
	curl -L --progress-bar -o apache-tomcat-${TOMCAT_VERSION}.tar.gz https://downloads.apache.org/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
	cd /usr/local/ && \
	echo "adding tomcat user ..." && \
	useradd -p tomcat -m tomcat && \
	echo "extracting ..." && \
	tar -zxf /apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
	echo "JAVA_HOME=${JAVA_HOME}" >> apache-tomcat-${TOMCAT_VERSION}/bin/setenv.sh && \
	echo "export JAVA_HOME" >> apache-tomcat-${TOMCAT_VERSION}/bin/setenv.sh && \
	echo "UMASK=\"0022\"" >> apache-tomcat-${TOMCAT_VERSION}/bin/setenv.sh && \
	echo "export UMASK" >> apache-tomcat-${TOMCAT_VERSION}/bin/setenv.sh && \
	chmod +x apache-tomcat-${TOMCAT_VERSION}/bin/setenv.sh && \
	sed -i 's/<\/tomcat-users>/<role rolename="admin-gui" \/>\n<role rolename="manager-gui" \/>\n<role rolename="manager-script" \/>\n<role rolename="manager-jmx" \/>\n<role rolename="manager-status" \/>\n<user username="tomcat" password="tomcat" roles="admin-gui,manager-gui,manager-script,manager-jmx,manager-status" \/>\n<\/tomcat-users>/' apache-tomcat-${TOMCAT_VERSION}/conf/tomcat-users.xml && \
	sed -i 's/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"/' apache-tomcat-${TOMCAT_VERSION}/webapps/manager/META-INF/context.xml && \
	sed -i 's/allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1" \/>/allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1" \/> -->/' apache-tomcat-${TOMCAT_VERSION}/webapps/manager/META-INF/context.xml && \
	sed -i 's/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"/' apache-tomcat-${TOMCAT_VERSION}/webapps/host-manager/META-INF/context.xml && \
	sed -i 's/allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1" \/>/allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1" \/> -->/' apache-tomcat-${TOMCAT_VERSION}/webapps/host-manager/META-INF/context.xml && \
	chown tomcat.tomcat apache-tomcat-${TOMCAT_VERSION} -R && \
	echo "creating links ..." && \
	ln -s apache-tomcat-${TOMCAT_VERSION} tomcat && \
	echo "removing downloaded tomcat ..." && \
	rm /apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
	echo " " && \
	/usr/local/tomcat/bin/version.sh && \
	echo " " && \
	echo "tomcat setup done"

EXPOSE 8080

CMD ["su", "-", "tomcat", "-c", "/usr/local/tomcat/bin/catalina.sh run"]

