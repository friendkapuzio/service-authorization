FROM openjdk:8
ENV APP_HOME=/root/dev/service-uat/
WORKDIR $APP_HOME
COPY build.gradle build-quality.gradle gradlew gradlew.bat $APP_HOME
COPY gradle $APP_HOME/gradle
COPY . .
RUN ./gradlew build -ParchiveName=service-authorization.jar -x findbugsMain -x javadoc -x jacocoTestReport

FROM openjdk:8-jre-alpine
MAINTAINER Andrei Varabyeu <andrei_varabyeu@epam.com>

ENV JAVA_OPTS="-Xmx512m -Djava.security.egd=file:/dev/./urandom"
ENV JAVA_APP=target/app.jar

WORKDIR /root/

RUN sh -c "echo $'#!/bin/sh \n\
exec java $JAVA_OPTS -jar $JAVA_APP' > /start.sh && chmod +x /start.sh"

VOLUME /tmp
COPY --from=0 /root/dev/service-uat/build/libs/lib target/lib/
COPY --from=0 /root/dev/service-uat/build/libs/service-authorization-3.3.0-SNAPSHOT.jar $JAVA_APP

RUN sh -c 'touch $JAVA_APP'
EXPOSE 8080
ENTRYPOINT ["/start.sh"]