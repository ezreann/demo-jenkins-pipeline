pipeline {

  agent {

    docker {

      image 'node:20-alpine'

      reuseNode true

    }

  }

  options {

    timestamps()

    ansiColor('xterm')

  }

  stages {

    stage('Checkout') { steps { checkout scm } }

    stage('Install')  { steps { sh 'npm ci || npm install' } }

    stage('Test')     { steps { sh 'npm test' } }

    stage('Publish test results') {

      steps { junit allowEmptyResults: false, testResults: 'reports/junit.xml' }

    }

  }

  post {

    success { echo 'Pipeline SUCCESS ?' }

    failure { echo 'Pipeline FAILED ? ? consultez les rapports JUnit.' }

    always  { archiveArtifacts artifacts: 'reports/**/*.xml', onlyIfSuccessful: false, allowEmptyArchive: true }

  }

}

