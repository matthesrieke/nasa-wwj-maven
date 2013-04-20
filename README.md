# Maven setup for Nasa Worldwind 4 Java (WWJ) 1.5.

As WWJ is currently not available in public maven repositories and in
general does not provide a maven-managed project setup, I decided to
create a maven project skeleton to allow wwj to be included into your
local maven repository.

This could also be used as a basis for a migration to Maven (3) by the
official WWJ project.

## Instructions

No .jar files are included in this project, but the pom consists of a
comfortable way of dealing with this situation. You only need to place the
worldwind-1.5.0.zip into the root of this project and run "mvn clean install".

## Insights

Using the maven-antrun-plugin, the archive is extracted and all .jar files
are copied to a dedicated local library repository (nasa-wwj/lib-repository).
Addiotnally, some natives are downloaded form jogamp mirrors. Check the pom.xml
for insights.

## Include in your Project

A dependecy would look like:
<dependency>
	<groupId>gov.nasa</groupId>
	<artifactId>wwj</artifactId>
	<version>1.5.0-SNASHOT</version>
</dependency>

