[![Maven Central](https://maven-badges.sml.io/sonatype-central/org.eximeebpms/eximeebpms-release-parent/badge.svg)](https://maven-badges.sml.io/sonatype-central/org.eximeebpms/eximeebpms-release-parent)

eximeebpms-release-parent
======================

Pom which can be inherited for eximeebpms releases defining some common release properties.
It allows to deploy to two repositories simultaneously. One is a Nexus OSS server, the other one a Nexus Enterprise server.
It will deploy the artifacts at the end of the build to keep the window of failure small when talking to external systems.

Usage
-----
TEST
Inherit the eximeebpms-release-parent pom inside your project like so  
  
    <parent>
      <groupId>org.eximeebpms</groupId>
      <artifactId>eximeebpms-release-parent</artifactId>
      <version>${LATEST_VERSION}</version>
    </parent>  
    
If you have a multi-module build, just inherit in your parent pom.  

Specify the ```<scm>``` section for your project eg.
```
<scm>
  <url>https://github.com/EximeeBPMS/MY_PROJECT_URL</url>
  <developerConnection>scm:git:https://github.com/EximeeBPMS/MY_PROJECT_URL.git</developerConnection>
  <tag>HEAD</tag>
</scm>
```
Release
-------

Prerequisite:  

  Add the following to the profiles and servers section of your local settings.xml file. This allows the signing of your artifacts during the release. <strong>Mandatory for maven central</strong>.
  
    <settings>
      ...
      
      <servers>
        ...

        <servers>
          <server>
            <id>central</id>
            <username>${CPP_USERNAME}</username>
            <password>${CPP_PASSWORD}</password>
          </server>
          <server>
            <id>github</id> #Your credentail to github 
            <username>${GITHUB_ACTOR}</username>
            <password>${GITHUB_TOKEN}</password>
          </server>        
      </servers>    
    </settings>

To release your own project use the following command:

    ./mvnw release:prepare release:perform -B \
        -Prelease \
        -DtagNameFormat=v@{project.version} \
        -DreleaseVersion=1.0.0 \
        -DdevelopmentVersion=1.1.0-SNAPSHOT \
    
This will trigger the `release` profile inside the `eximeebpms-release-parent` pom automatically.
