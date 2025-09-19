"""))

write(os.path.join(maven_dir, "pom.xml"), textwrap.dedent("""
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>com.example</groupId>
<artifactId>demo</artifactId>
<version>1.0-SNAPSHOT</version>

<properties> <maven.compiler.source>17</maven.compiler.source> <maven.compiler.target>17</maven.compiler.target> <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> <junit.jupiter.version>5.9.2</junit.jupiter.version> </properties> <dependencies> <dependency> <groupId>org.junit.jupiter</groupId> <artifactId>junit-jupiter</artifactId> <version>${junit.jupiter.version}</version> <scope>test</scope> </dependency> </dependencies> <build> <plugins> <plugin> <groupId>org.apache.maven.plugins</groupId> <artifactId>maven-surefire-plugin</artifactId> <version>3.0.0-M7</version> <configuration> <useModulePath>false</useModulePath> </configuration> </plugin> </plugins> </build> </project> """))

write(os.path.join(maven_dir, "src/main/java/App.java"), textwrap.dedent("""
public class App {
public static int add(int a, int b) {
return a + b;
}
public static void main(String[] args) {
System.out.println("Hello from App: " + add(1, 2));
}
}
"""))

write(os.path.join(maven_dir, "src/test/java/AppTest.java"), textwrap.dedent("""
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class AppTest {
@Test
void testAdd() {
assertEquals(3, App.add(1, 2));
}
}
"""))

write(os.path.join(maven_dir, "Jenkinsfile"), textwrap.dedent("""
pipeline {
agent {
docker { image 'maven:3.9.9-eclipse-temurin-17' }
}
options {
timestamps()
ansiColor('xterm')
}
stages {
stage('Checkout') {
steps {
checkout scm
}
}
stage('Build & Test') {
steps {
sh 'mvn -q -B -DskipTests=false test'
}
}
stage('Publish test results') {
steps {
junit allowEmptyResults: false, testResults: 'target/surefire-reports/.xml'
}
}
}
post {
success {
echo 'Pipeline SUCCESS ?'
}
failure {
echo 'Pipeline FAILED ? ? vérifiez les logs et les rapports JUnit.'
}
always {
archiveArtifacts artifacts: 'target/surefire-reports/.xml', onlyIfSuccessful: false, allowEmptyArchive: true
}
}
}
"""))