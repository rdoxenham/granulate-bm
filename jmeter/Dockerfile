FROM registry.access.redhat.com/ubi8/ubi:latest as build

RUN dnf install wget -y \
        && wget https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.5.tgz -P /opt \
        && cd /opt \
        && tar -xvf apache-jmeter-5.5.tgz \
        && rm apache-jmeter-5.5.tgz

RUN wget "https://search.maven.org/remotecontent?filepath=com/github/johrstrom/jmeter-prometheus-plugin/0.6.0/jmeter-prometheus-plugin-0.6.0.jar" \
	-O /opt/apache-jmeter-5.5/lib/ext/jmeter-prometheus-plugin-0.6.0.jar

RUN echo "server.rmi.ssl.disable=true" >> /opt/apache-jmeter-5.5/bin/user.properties
RUN echo "prometheus.ip=0.0.0.0" >> /opt/apache-jmeter-5.5/bin/user.properties

FROM registry.access.redhat.com/ubi8/openjdk-17

VOLUME /inputs

COPY --from=build --chown=jboss:0 /opt/apache-jmeter-5.5 .

CMD sh /home/jboss/bin/jmeter.sh -n -t /inputs/simulation.jmx
