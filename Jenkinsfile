pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building branch ${env.BRANCH_NAME}"
                sh 'echo Build successful'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests"
                sh 'echo Tests passed'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying from main branch"
            }
        }

        stage('Release Build') {
            when {
                expression { env.BRANCH_NAME.startsWith("release") }
            }
            steps {
                echo "Running release logic"
            }
        }

        stage('Feature Validation') {
            when {
                expression { env.BRANCH_NAME.startsWith("feature") }
            }
            steps {
                echo "Running feature validation"
            }
        }
    }
}