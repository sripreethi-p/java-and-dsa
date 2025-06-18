# Experienced Hacks to Fix Bugs

1. [To pick the required Java SDK in Gradle project](#to-pick-the-required-java-sdk-in-gradle-project)
2. [To enable logging in Gradle project](#to-enable-logging-in-gradle-project)




### To pick the required Java SDK in Gradle project
If the project isn't picking the required java sdk, add the below in build.gradle file
```java
tasks.withType(JavaCompile) {
    options.fork = true
    options.forkOptions.javaHome = file("/Users/preethi/.jenv/versions/temurin64-21.0.4")
}
```  
<br></br>
  
### To enable logging in Gradle project
If `org.slf4j.LoggerFactory` isn't printing logs, add the below dependencies in build.gradle file.  
**Reason:** `org.slf4j.LoggerFactory` is a part of the SLF4J (Simple Logging Facade for Java) API, which provides a simple abstraction for various logging frameworks. 
However, SLF4J itself does not provide any logging implementation. To actually see log output, you need to include a concrete logging implementation in your project.
There are many logging implementations available, such as Logback, Log4j, and java.util.logging.
  - Logback:  
  🟢 Best for general-purpose logging, as it is the native implementation of SLF4J and offers a good balance of performance and features.  
  🟢 Highly configurable and supports advanced features like filtering, rolling policies, and more.  
  🔴 Slightly less performant compared to Log4j2 for high-throughput applications.  
  - Log4j2:  
  🟢 Best for high-performance logging, especially in applications with very high logging throughput.  
  🟢 Offers advanced features like asynchronous logging, custom log levels, and more.  
  🔴 More complex configuration compared to Logback.
  - java.util.logging:  
  🟢 Best for simple applications or when you want to avoid adding external dependencies.  
  🟢 Part of the standard Java library, so no additional setup is required.  
  🔴 Less powerful and flexible compared to Logback and Log4j2.
  - TinyLog:  
  🟢 Best for lightweight applications or when you want a minimal logging solution.  
  🟢 Very simple to set up and use.  
  🔴 Lacks advanced features and flexibility compared to other logging frameworks.
  -  Apache Commons Logging:  
  🟢 Best for applications that need to support multiple logging frameworks.  
  🟢 Provides a simple abstraction layer over various logging implementations.  
  🔴 Adds a layer of complexity and may introduce performance overhead.  
  
  💡 Which one to choose?

Choose **Log4j2** if **performance and advanced features** (e.g., asynchronous logging) are critical.
```toml
[versions]
        slf4j = "2.0.9"
        logback = "1.4.11"

[libraries]
        slf4j-api = { group = "org.slf4j", name = "slf4j-api", version.ref = "slf4j" }
        logback-classic = { group = "ch.qos.logback", name = "logback-classic", version.ref = "logback" }

[bundles]
        logging = ["slf4j-api", "logback-classic"]
```

Choose **Logback** if **simplicity and SLF4J compatibility** are more important.
```toml
[versions]
            slf4j = "2.0.9"
            log4j = "2.20.0"

[libraries]
            slf4j-api = { group = "org.slf4j", name = "slf4j-api", version.ref = "slf4j" }
            log4j-core = { group = "org.apache.logging.log4j", name = "log4j-core", version.ref = "log4j" }
            log4j-slf4j-impl = { group = "org.apache.logging.log4j", name = "log4j-slf4j-impl", version.ref = "log4j" }

[bundles]
            logging = ["slf4j-api", "log4j-core", "log4j-slf4j-impl"]
```
For most modern projects, Logback is sufficient unless you have specific performance needs.

