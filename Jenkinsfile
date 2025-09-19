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