# Spring Boot Docker - Exploded

A layer caching efficient approach for building Spring boot application's docker image. This approach explodes the generated uber jar and then packages each layer individually. 

**Step 1:** Configure [maven-dependency-plugin's unpack](https://maven.apache.org/plugins/maven-dependency-plugin/unpack-mojo.html) goal in the pom. This will unpack the uber jar in `target/dependency` folder.


**Step 2:** Maven build the jar file - `./mvnw package`

**Step 3:** The [Dockerfile](./Dockerfile) packages components from dependency folder. This creates a multi-layer structure in image.

**Step 3:** Build image - `docker build -t spring-boot-docker-exploded .`

**Step 4:** Run the app - `docker run -p 8080:8080 --rm -it -name basic spring-boot-docker-exploded`


**About this approach:**

This is much better approach than [basic](../basic). It can efficiently cache dependency libs into separate layers. 

```shell
docker history spring-boot-docker-exploded 
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
f82e27d65508   9 minutes ago   ENTRYPOINT ["java" "-cp" "app:app/lib/*" "co…   0B        buildkit.dockerfile.v0
<missing>      9 minutes ago   COPY target/dependency/BOOT-INF/classes /app…   1.4kB     buildkit.dockerfile.v0
<missing>      9 minutes ago   COPY target/dependency/META-INF /app/META-IN…   2.72kB    buildkit.dockerfile.v0
<missing>      9 minutes ago   COPY target/dependency/BOOT-INF/lib /app/lib…   17.5MB    buildkit.dockerfile.v0
<missing>      9 minutes ago   ARG DEPENDENCY=target/dependency                0B        buildkit.dockerfile.v0
<missing>      9 minutes ago   USER spring:spring                              0B        buildkit.dockerfile.v0
<missing>      6 hours ago     RUN /bin/sh -c addgroup -S spring && adduser…   4.7kB     buildkit.dockerfile.v0

```
The history shows a separate layer of lib and it constitutes the ~98% of the size. If dependencies are not changed, any source code change will not result in rebuilding that lib layer. Thus saving the storage as well as network cost.

CAUTION: The application's main class is coupled to the Dockerfile.

NOTE: Prefer this approach over basic approach.


