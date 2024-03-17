![image](https://github.com/SrikanthNoone/cicd-maven-jenkins-docker/assets/97281147/6e00417d-acfb-4912-bb50-cd97a957398d)# cicd-maven-jenkins-docker

this project is done in following steps:
1. created a simple HTML web page of ecommerce website.
2. Added a Dockerfile to create a docker image
3. Added a pom.xml file for maven building
4. WEB-INF file is also added as dependency of maven plugin
The structure of reo in git is:
  src/main/webapp/ in this directory WEB-INF folder and index.html file added
  in WEB-INF folder web.xml file added
  in root directory (master) along with src folder - Dockerfile and pom.xml added

--Then followed simple steps to clone the git repo into ubuntu instance
--Added maven plugin in jenkins 
--Added docker plugin in jenkins
--maven war file created in instance
--docker image created on base image of tomcat server
--docker container created and accessed via port number 9090
================================================================
web.xml file:
------------------
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
</web-app>

==================================================================
pom.xml file:
-------------
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>srikanth</groupId>
  <artifactId>01-maven-new</artifactId>
  <packaging>war</packaging>
  <version>3.0-RELEASE</version>
  <name>01-maven-new</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
    </dependency>
  </dependencies>
  
  <build>
    <finalName>maven-new</finalName>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.3.1</version>
      </plugin>
    </plugins>
  </build>
</project>
=====================================================================

Dockerfile:
------------
FROM tomcat:8.0.20-jre8
MAINTAINER Srikanth <ashok@oracle.com>
EXPOSE 8080
COPY target/maven-new.war /usr/local/tomcat/webapps/maven-new.war
========================================================================

index.html:
------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My eCommerce Website</title>
    <!-- Link to your CSS file for styling -->
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Header -->
    <header>
        <h1>Buy your product here!!!!</h1>
        <h2>your need ends here</h2>
        <div class="logo">Your Logo</div>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">Shop</a></li>
                <li><a href="#">About Us</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
        <div class="cart">Your Cart (0)</div>
    </header>

    <!-- Main Content -->
    <main>
        <!-- Featured Products Section -->
        <section class="featured-products">
            <h2>Featured Products</h2>
            <div class="product">
                <img src="product1.jpg" alt="Product 1">
                <h3>Product Name</h3>
                <p>Description of the product.</p>
                <button>Add to Cart</button>
            </div>
            <!-- Repeat the above product structure for other featured products -->
        </section>

        <!-- Categories Section -->
        <section class="categories">
            <h2>Categories</h2>
            <ul>
                <li><a href="#">Category 1</a></li>
                <li><a href="#">Category 2</a></li>
                <li><a href="#">Category 3</a></li>
                <!-- Add more categories as needed -->
            </ul>
        </section>
    </main>

    <!-- Footer -->
    <footer>
        <p>&copy; 2024 Your Company</p>
    </footer>
</body>
</html>
=======================================================================

Jenkins pipeline script:
---------------------------
pipeline {
    agent any
    tools{
        maven 'mvn3'
    }

    stages {
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/SrikanthNoone/maven-new.git'
            }
        }
        stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }
         stage('docker build') {
            steps {
                sh 'docker build -t image .'
            }
        }
        stage('docker container') {
            steps {
                sh 'docker stop image-container'
                sh 'docker rm image-container'
                sh 'docker run -d -p 9090:8080 --name image-container image'
            }
        }
    }
}
==============================================================================



Successfully accessed web app from docker container..... 
