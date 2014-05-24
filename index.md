---
layout: default
---

## Overview

The Scassandra Java Client is in Maven central. You can add it as a depdency:

### Gradle:

~~~ groovy
dependencies {
testCompile 'org.scassandra:java-client:0.1'
}
~~~

### Maven:

~~~ xml
<dependency>
  <groupId>org.scassandra</groupId>
  <artifactId>java-client</artifactId>
  <version>0.1</version>
  <scope>test</scope>
</dependency>
~~~
If you have dependency clashes with Guava, Apache Http Client etc try the standalone version:

~~~ xml
<dependency>
  <groupId>org.scassandra</groupId>
  <artifactId>java-client</artifactId>
  <version>0.1</version>
  <classifier>standalone</classifier>
  <scope>test</scope>
</dependency>
~~~

### Example project

To see how to use Scassandra in your Java unit and integration tests see the following example project:

[Scassandra Java Example](https://github.com/chbatey/scassandra-example-java)

And particularly the ExampleDaoTest:

[Example JUnit test](https://github.com/chbatey/scassandra-example-java/blob/master/src/test/java/com/batey/examples/scassandra/PersonDaoTest.java)