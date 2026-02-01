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
        echo "Building branch ${env.BRANCH_NAME}"
        sh 'echo Build successful'
    }
}

    stage('Test') {
      steps {
        sh 'mvn -B -ntp test'
      }
    }

    stage('Deploy') {
      when {
        branch 'main'
      }
      steps {
        echo 'Deploying to production'
      }
    }

    stage('Release Build') {
      when {
        expression { return env.BRANCH_NAME?.startsWith('release/') }
      }
      steps {
        echo 'Release branch build logic'
      }
    }

    stage('Feature Validation') {
      when {
        expression { return env.BRANCH_NAME?.startsWith('feature/') }
      }
      steps {
        echo 'Feature branch build only'
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'target/**', allowEmptyArchive: true
    }
  }
}
