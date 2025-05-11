FROM tomcat:8.5.72-jdk8-openjdk-buster

ENV MAVEN_HOME=/usr/share/maven
ENV MAVEN_VERSION=3.8.4

# Install Maven
RUN apt-get update && \
    apt-get install -y curl && \
    curl -fsSL https://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
    | tar xzf - -C /usr/share && \
    mv /usr/share/apache-maven-${MAVEN_VERSION} /usr/share/maven && \
    ln -s /usr/share/maven/bin/mvn /usr/bin/mvn && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy project files
COPY ./pom.xml /app
COPY ./src /app/src
COPY ./settingscopy.xml /app/settings.xml

# Build the project
RUN mvn package 

# Deploy WAR file to Tomcat
RUN cp /app/target/addressbook.war /usr/local/tomcat/webapps/

# Expose default Tomcat port
EXPOSE 8080

# Start Tomcat
CMD ["catalina.sh", "run"]
