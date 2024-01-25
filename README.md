# gradle-variable-bug

This project reproduces a bug with Quarkus and using gradle variables in application properties.

This example has a variable set in `gradle.properties` called `newRelicVersion`.

In `application.properties` this variable is used to set the version in the path to a jar file

However, when building an image and running a container the gradle variable is not substituted, and instead it falls back to the default value

This issue is happening in Quarkus version `3.6.5+`

### To reproduce:
 1. Build an image: ```./gradlew build```
 2. Run container: ```docker run gradle-variable-bug:1.0-SNAPSHOT```
 3. You should see the following error: 
 ```
Error opening zip file or JAR manifest missing : lib/main/com.newrelic.agent.java.newrelic-agent-unknown.jar
```
It is falling back to the default value of `unknown` for `newRelicVersion` instead of picking up the value set in `gradle.properties`

 4. Switch Quarkus version to `3.6.4` in `gradle.properties`
 5. Follow steps 1 and 2 again
 6. You should not see the error and the container will successfully start