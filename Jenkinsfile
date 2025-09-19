pipeline {

  agent {

    docker {

      image 'node:20-alpine'      // Environnement d'exécution

      reuseNode true              // Réutilise le même workspace

    }

  }



  options {

    timestamps()                  // Ajoute l'heure dans les logs

    ansiColor('xterm')            // Couleurs dans les logs (si plugin installé)

  }



  stages {



    stage('Checkout') {

      steps {

        echo "?? Récupération du code source"

        checkout scm

      }

    }



    stage('Install Dependencies') {

      steps {

        echo "?? Installation des dépendances"

        sh 'npm ci || npm install'

      }

    }



    stage('Run Unit Tests') {

      steps {

        echo "?? Exécution des tests unitaires"

        sh 'npm test'

      }

    }



    stage('Publish Test Results') {

      steps {

        echo "?? Publication des rapports JUnit"

        junit allowEmptyResults: false, testResults: 'reports/junit.xml'

      }

    }



    // Optionnel : intégration SquashTM si tu as un serveur configuré

    /*

    stage('Send Results to Squash TM') {

      steps {

        echo "?? Envoi des résultats à Squash TM"

        squashTMUpload(

          squashTMServerName: 'SquashTM-Local',

          testResultFile: 'reports/junit.xml',

          testPlanId: '123',

          iterationId: '456'

        )

      }

    }

    */

  }



  post {

    always {

      echo '?? Nettoyage / Fin de pipeline'

      archiveArtifacts artifacts: 'reports/**/*.xml', allowEmptyArchive: true

    }

    success {

      echo '? Pipeline réussi ! Tous les tests sont passés.'

    }

    failure {

      echo '? Pipeline échoué. Vérifiez les logs et les rapports JUnit.'

    }

  }

}

