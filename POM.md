# POM

##### POM Properties

```
<properties>


        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>

        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

        <failOnMissingWebXml>false</failOnMissingWebXml>
        <spring.version>5.1.0.RELEASE</spring.version>
        <hibernate.version>5.3.5.Final</hibernate.version>
        <hibernate.validator>5.4.1.Final</hibernate.validator>
        <c3p0.version>0.9.5.2</c3p0.version>
        <jstl.version>1.2.1</jstl.version>
        <tld.version>1.1.2</tld.version>
        <servlets.version>3.1.0</servlets.version>
        <jsp.version>2.3.1</jsp.version>
        <hsqldb.version>1.8.0.10</hsqldb.version>

    </properties>

```

##### POM BUILD

```
<build>

<!-- MAVEN COMPILER -->

<plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

<!-- MAVEN WAR PLUGIN -->

   <plugin>
    <artifactId>maven-war-plugin</artifactId>
    <version>2.0</version>
    <configuration>
     <webResources>
      <resource>
       <directory>WebContent</directory>
      </resource>
     </webResources>
    </configuration>
    </plugin>

</plugins>

</build>

```

##### REPOSITORY

```
<repositories>
    <!-- This repository is where the spring framework dependencies jar file download. -->
        <repository>
            <id>spring-snapshots</id>
            <name>Spring Snapshots</name>
            <url>https://repo.spring.io/libs-snapshot</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
</repositories>

```

