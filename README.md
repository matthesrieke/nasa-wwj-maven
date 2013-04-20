# Maven setup for Nasa Worldwind 4 Java (WWJ) 1.5.

As WWJ is currently not available in public maven repositories and in
general does not provide a maven-managed project setup, I decided to
create a maven project skeleton to allow wwj to be included into your
local maven repository.

This could also be used as a basis for a migration to Maven (3) by the
official WWJ project.

## Instructions

No .jar files are included in this project, but the POM consists of a
comfortable way of dealing with this situation. You only need to place the
[worldwind-1.5.0.zip](http://worldwind.arc.nasa.gov/java/) into the root of this project and run `mvn clean install`.

## Insights

Using the maven-antrun-plugin, the worldwind release archive is extracted and all .jar files
are copied to a dedicated local library repository (nasa-wwj/lib-repository).
Additionally, some natives are downloaded form jogamp mirrors. Check the pom.xml
for insights.

## Include in your Project

A dependecy would look like:

```xml
<dependency>
	<groupId>gov.nasa</groupId>
	<artifactId>wwj</artifactId>
	<version>1.5.0-SNASHOT</version>
</dependency>
```

## Example pom.xml

This POM can serve as a skeleton for a project. It includes unpacking
of native libraries (e.g. jogl and gluegen) and creating executable scripts
for a built-in WWJ example application (placed in target/appassembler).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	
	<groupId>my.wwj.project</groupId>
	<artifactId>wwj-skeleton</artifactId>
	<version>1.0-SNAPSHOT</version>

	<name>aixm4wwj-skeleton</name>

	<properties>
		<assemble.dir>${project.build.directory}/appassembler</assemble.dir>
		<wwj.version>1.5.0-SNAPSHOT</wwj.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.8.1</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>gov.nasa</groupId>
			<artifactId>wwj</artifactId>
			<version>${wwj.version}</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.3.2</version>
				<configuration>
					<source>1.6</source>
					<target>1.6</target>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>appassembler-maven-plugin</artifactId>
				<version>1.3</version>
				<executions>
					<execution>
						<goals>
							<goal>assemble</goal>
						</goals>
						<phase>package</phase>
						<configuration>
							<programs>
								<program>
									<mainClass>gov.nasa.worldwindx.examples.Airspaces</mainClass>
									<name>MyWWJProject</name>
								</program>
							</programs>
							<extraJvmArguments>-Djava.library.path=../lib/natives</extraJvmArguments>
							<assembleDirectory>${assemble.dir}</assembleDirectory>
							<repositoryName>lib</repositoryName>
							<repositoryLayout>flat</repositoryLayout>
							<useWildcardClassPath>true</useWildcardClassPath>
							<binFolder>bin</binFolder>
							<binFileExtensions>
								<unix>.sh</unix>
							</binFileExtensions>
						</configuration>
					</execution>
				</executions>
			</plugin>
			<plugin>
				<groupId>com.googlecode.mavennatives</groupId>
				<artifactId>maven-nativedependencies-plugin</artifactId>
				<version>0.0.7</version>
				<configuration>
					<nativesTargetDir>${assemble.dir}/lib/natives</nativesTargetDir>
					<separateDirs>false</separateDirs>
				</configuration>
				<executions>
					<execution>
						<id>unpacknatives</id>
						<goals>
							<goal>copy</goal>
						</goals>
						<phase>package</phase>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

</project>
```
