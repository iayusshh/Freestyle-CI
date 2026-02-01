pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building branch: ${env.BRANCH_NAME}"
                sh 'mvn -B -ntp clean compile'
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
                sh 'echo Deploying application'
            }
        }

        stage('Release Build') {
            when {
                expression { return env.BRANCH_NAME?.startsWith('release/') }
            }
            steps {
                echo 'Running release logic'
            }
        }

        stage('Feature Validation') {
            when {
                expression { return env.BRANCH_NAME?.startsWith('feature/') }
            }
            steps {
                echo 'Running feature validation'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}