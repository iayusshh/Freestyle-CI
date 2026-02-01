pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn -B -ntp -DskipTests package'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'target/**', allowEmptyArchive: true
    }
  }
}
