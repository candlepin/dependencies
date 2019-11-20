# Candlepin Dependencies
Package Registry for Candlepin dependencies using the Git Hub Package Registry API.

# Uploading a dependency

1. Create a [personal access token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) for yourself with the `read:packages` and `write:packages` scope
2. Follow the [Configuring Maven for use with GitHub Package Registry](https://help.github.com/en/github/managing-packages-with-github-package-registry/configuring-apache-maven-for-use-with-github-package-registry) document and set up a `~/.m2/settings.xml` file.  It should look something like this:

   ```xml
   <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">

     <activeProfiles>
       <activeProfile>github</activeProfile>
     </activeProfiles>

     <profiles>
       <profile>
         <id>github</id>
         <repositories>
           <repository>
             <id>central</id>
             <url>https://repo1.maven.org/maven2</url>
             <releases><enabled>true</enabled></releases>
             <snapshots><enabled>true</enabled></snapshots>
           </repository>
           <repository>
             <id>github</id>
             <name>GitHub Candlepin Apache Maven Packages</name>
             <url>https://maven.pkg.github.com/candlepin/dependencies</url>
           </repository>
         </repositories>
       </profile>
     </profiles>

     <servers>
       <server>
         <id>github</id>
         <username>YOUR_GITHUB_NAME_HERE</username>
         <password>YOUR_TOKEN_HERE</password>
       </server>
     </servers>
   </settings>
   ```
3. Upload new dependencies using a command like

   ```
   $ mvn deploy:deploy-file -Dfile=/tmp/jss-4.5.0.jar -DrepositoryId=github -Durl=https://maven.pkg.github.com/candlepin/dependencies -DgroupId=org.mozilla -DartifactId=jss -Dversion=4.5.0 -Dpackaging=jar
   ```

   Note that you need to provide the groupId, artifactId, version, and packaging yourself.  Also note that the file is not
   being uploaded directly from the location in the local maven repository (`~/.m2/repository/`).  There is a [bug in Maven]
   (https://stackoverflow.com/a/26411581/6124862) where `deploy-file` doesn't work properly when given a path within the
   local repository.  Just copy the file to `/tmp` and upload from there.
