# Spring Boot Docker - Basic 

A simple approach to dockerize a spring boot application.

**Step 1:** Maven build the jar file - `./mvnw package`

**Step 2:** The [Dockerfile](./Dockerfile) that packages generated jar file into docker image.

**Step 3:** Build image - `docker build -t spring-boot-docker-basic .`

**Step 4:** Run the app - `docker run -p 8080:8080 --rm -it -name basic spring-boot-docker-basic`


**About this approach:**

This is a very simple docker build file that adds the spring boot application's uber jar into docker image.

```shell
docker history spring-boot-docker-basic 
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
7ca1fc4cdaf0   2 hours ago   ENTRYPOINT ["java" "-jar" "app.jar"]            0B        buildkit.dockerfile.v0
<missing>      2 hours ago   COPY target/*.jar app.jar # buildkit            17.6MB    buildkit.dockerfile.v0
<missing>      2 hours ago   WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      2 hours ago   ARG JAR_FILE=target/*.jar                       0B        buildkit.dockerfile.v0
<missing>      2 hours ago   USER spring:spring                              0B        buildkit.dockerfile.v0
<missing>      2 hours ago   RUN /bin/sh -c addgroup -S spring && adduser…   4.7kB     buildkit.dockerfile.v0
```
The application jar is added into a single layer. This is **bad** for layer caching. The example application is very simple, just one `GET /greet` endpoint. Still, the jar size is 17 MB. Making any change in the code will cause a new layer to be generated and stored in the registry. 

In real production application, the size of jar can easily get bigger due to dependencies. Even though the source code can change, the dependencies will rarely change. Dependencies usually constitutes the majority of the application jar size.

CAUTION: This approach is very inefficient for image caching and storage.


