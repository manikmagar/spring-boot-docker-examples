# Spring Boot Docker - Basic Maven Build Plugin

Same approach as described in [Basic](../basic/README.md). This example uses [dockerfile-maven plugin](https://github.com/spotify/dockerfile-maven) to build docker image as a part of maven build cycle. 

See plugin configuration in [pom.xml](./pom.xml).

**About this approach:**

The dockerfile-maven plugin _**can be applied to any approach**_ as it only helps to trigger docker build during maven lifecycle. It simplifies the build (tied to mvn package phase) and push (tied to mvn deploy phase) of the docker image. 


