FROM fedora:latest

MAINTAINER "msjkus"
LABEL org.opencontainers.image.authors="msjkus"

RUN	echo "downloading jdk ..." && \
	curl -L --progress-bar --retry 10 --retry-delay 15 -o openjdk-11.0.2_linux-x64_bin.tar.gz https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz && \
	cd /usr/local/ && \
	echo "extracting ..." && \
	tar -zxf /openjdk-11.0.2_linux-x64_bin.tar.gz && \
	echo "creating links ..." && \
	ln -s jdk-11.0.2 jdk && \
	echo "removing downloaded ..." && \
	rm /openjdk-11.0.2_linux-x64_bin.tar.gz && \
	echo " " && \
	/usr/local/jdk/bin/java --version && \
	echo " " && \
	echo "jdk setup done"

ENV JAVA_HOME=/usr/local/jdk

ENV PATH=${JAVA_HOME}/bin:$PATH

CMD ["/bin/bash"]

