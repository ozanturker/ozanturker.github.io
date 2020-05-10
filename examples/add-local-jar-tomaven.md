# Add Local .jars to Maven

Assume that you have some jar files and you want to build Java project with maven but jars not exist in maven central repository. How do you add your jars into projects? Example shown below.

```xml
        <dependency>
            <groupId>jar_group_id</groupId>
            <artifactId>artifact_id</artifactId>
            <scope>system</scope>
            <version>version_number</version>
            <systemPath>${project.basedir}/jar_path</systemPath>
        </dependency>
```

