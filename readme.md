## Description

This pom is a default Pom useful for all  projects.

To use it, simple do this : 

1. declare this parent (latest version is better) : 

```xml
	<parent>
		<groupId>beepee37.common</groupId>
		<artifactId>default-maven-parent</artifactId>
		<version>1.0.0</version>
		<relativePath />
	</parent>
```
2.	You can activate your current JDK doing this: 

	- if your are still using jdk8 for running maven, set your java version in the properties

```xml:
	<properties>
		<java.source.version>1.8</java.source.version>
		<java.target.version>1.8</java.target.version>
	</properties>
```
   

	- if your are using jdk for running maven, set your java release version the properties (have in mind that you can generated java8 binaries with jdk 11 for example)
   
```xml:
	<properties>
		<maven.compiler.release>8</maven.compiler.release>
	</properties>
```

## JAVA_8_Mvn_Env
The specific JAVA_8_Mvn_Env configuration is activated automatically if maven is run with jdk8.
   

## JAVA_11ON_Mvn_Env
The specific JAVA_11ON_Mvn_Env configuration is activated automatically if maven is run with jdk11 or higher.

## Testing
For enabling testing in your applications build, ``JAVA_11_TESTS`` or ``JAVA_8_TESTS`` profile are available.
Normally one of this profiles is automatically activated in this parent if you didn't activate the property  ``maven.test.skip`` in your command line or in the pom.xml properties section, depending on the jdk that is used to run maven (jdk 8 / jdk11).

### Provided testing capabilities

Tthis pom.xml provides unit and integration tests. The first one is run using [surefire plugin](https://maven.apache.org/surefire/maven-surefire-plugin/) in the ``test`` phase. The second one is run using [failsafe plugin](https://maven.apache.org/surefire/maven-failsafe-plugin/) in the ``integration-test`` phase.

## Code coverage 
To get code coverage figures in your quality dashboards such as in SONARQUBE, you have to reproduce the following steps in your project:

- Use at least the version of this project as parent in your pom.xml as mentioned above  

- Declare the maven-jacoco-plugin in your build process by adding this configuration: 

```xml
    <plugin>
	    <groupId>org.jacoco</groupId>
		<artifactId>jacoco-maven-plugin</artifactId>
	</plugin>
```

By using the ``jacoco-coverage`` profile, this plugin is automatically activated if you didn't declare the property  ``maven.test.skip`` in your command line or in the pom.xml properties section.

Jacoco execution is associated to surefire and failsafe executions to run both unit and integration tests.

The provided configuration initialized the ``argLine`` property by adding the ``jaCoCoArgLine`` property which has been set up by the jacoco prepare agent execution.

### Create report
Now you can create the report by using this kind of command:

```jshelllanguage
mvn clean  integration-test jacoco:report  -f ./wsdp-api-all/pom.xml
```

The report should be created on the ``target/site/jacoco``  folder for each module.

## SONAR
To run sonar, you have to execute maven sonar plugin. 

For instance :

```jshelllanguage
mvn sonar:sonar  -f ./wsdp-api-all/pom.xml
```

## Dependency Check 
To run dependency check, you have to execute maven dependency check plugin. 
This plugin has been added to the default goals of the maven build, but is executed only if  :
 - maven property  dependency-check.skip is explicitly set to false   
or
 - an environment variable named ACTIVATE_DEPENDENCY_CHECK has been defined and set to true   

You'll find the report in the target repository.

For instance :

```jshelllanguage
#if you want to build you project and generate dependency check report
mvn clean:install -Ddependency-check.skip=false
   
#if you want to skip test classes compilation and tests execution
mvn clean:install -Ddependency-check.skip=false -Dmaven.test.skip=true 
   
#if you want to compile test classes but not to execute tests
mvn clean:install -Ddependency-check.skip=false -DskipTests
```
