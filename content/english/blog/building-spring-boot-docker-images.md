---
title: "Building Spring Boot Docker Image"
meta_title: "Building Spring Boot Docker Images"
description: "A guide on how to build and run Docker images for a Spring Boot application."
date: 2023-05-03T13:52:00
categories: ["Java", "Spring", "Docker"]
author: "David"
tags: ["java", "spring", "docker"]
draft: false
---

**Pre-Requirements**

- Developer Environment ready with Docker, JDK, IDE
- A Java Spring Boot Project with a h2 in-memory DB
- Docker Hub account

## Create Docker File

Create a Dockerfile with the following content:

```dockerfile
FROM openjdk:11-jre-slim
ENV JAVA_OPTS " -Xms512m -Xmx512m -Djava.security.egd=file:///dev/./urandom"
WORKDIR application
COPY target/myapp-0.0.1-SNAPSHOT.jar ./
ENTRYPOINT ["java", "-jar", "myapp-0.0.1-SNAPSHOT.jar"]
```

Make sure the paths where the .jar files are stored match your environment.

## Build and Run the Docker Image

Build the image with this command:

```bash
docker build -f ./src/main/Dockerfile -t myapp .
```

Run the image with this command:

```bash
docker run -p 8080:8080 -d myapp
```

The Spring Application-Context should now be loaded and started inside the container. You can verify this with the `docker ps -a` and `docker logs -f <containername>` commands.

  ## Add Layer Tool in Maven

  Make sure that you have enabled the Layer Tool with the following content in the `pom.xml` file:

  ```xml
  <plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
      <layers>
        <enabled>true</enabled>
        <includeLayerTools>true</includeLayerTools>
      </layers>
      <excludes>
        <exclude>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
        </exclude>
      </excludes>
    </configuration>
  </plugin>
  ```

  Enabling the layer tool in a Java Spring project that runs in a Docker container can help reduce the size of the Docker image, improve the speed of deployment, and optimize resource usage.

  ## Enable Multi-Stage Dockerfile

  Create an example of a Multi-Stage Dockerfile:

  ```dockerfile
  FROM openjdk:11-jre-slim as builder
  WORKDIR application
  ADD target/myapp-0.0.1-SNAPSHOT.jar ./
  RUN java -Djarmode=layertools -jar myapp-0.0.1-SNAPSHOT.jar extract

  FROM openjdk:11-jre-slim
  WORKDIR application
  COPY --from=builder application/dependencies/ ./
  COPY --from=builder application/spring-boot-loader/ ./
  COPY --from=builder application/snapshot-dependencies/ ./
  COPY --from=builder application/application/ ./
  ENTRYPOINT ["java", "-Djava.security.egd=file:///dev/./urandom", "org.springframework.boot.loader.JarLauncher"]
  ```

  Recreate the image with:

  ```bash
  docker build -f ./src/main/docker/Dockerfile -t myapp .
  ```

  ## Build the Docker Image with Maven

  Add the Docker Maven plugin to your `pom.xml`:

  ```xml
  <plugin>
    <groupId>io.fabric8</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>0.42.1</version>
    <configuration>
      <verbose>true</verbose>
      <images>
        <image>
          <name>yourdockeraccount/myapp</name>
          <build>
            <assembly>
              <descriptorRef>artifact</descriptorRef>
            </assembly>
            <dockerFile>Dockerfile</dockerFile>
            <tags>
              <tag>latest</tag>
              <tag>${project.version}</tag>
            </tags>
          </build>
        </image>
      </images>
    </configuration>
  </plugin>
  ```

  Replace the artifact name `myapp-0.0.1-SNAPSHOT.jar` with `${project.build.finalName}.jar` in the Dockerfile.

  Build the Docker image with the Maven plugin using a dynamic version.

  ## Push your Docker Image to Docker Hub

  Build the image with:

  ```bash
  mvn clean package docker:build docker:push
  ```
