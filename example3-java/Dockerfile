FROM alpine
WORKDIR /root/hello-world
COPY code /root/hello-world

# Install JDK
RUN apk add openjdk8
ENV JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
ENV PATH="$JAVA_HOME/bin:${PATH}"

RUN javac HelloWorld.java

ENTRYPOINT java HelloWorld