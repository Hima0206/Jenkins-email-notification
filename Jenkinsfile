@Library('Jenkins-shared-library') _
pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sh "echo 'Checkout Completed'"
            }
        }
        stage('Gitleaks scan') {
            steps {
                sh '''
                docker run --rm -v "$(pwd)":/repo zricethezav/gitleaks:latest detect \
                  --no-git --source=/repo --exit-code=1
                '''
            }
        }
        stage('Build') {
            steps {
                sh "echo 'Build Completed'"
            }
        }
    }
    post {
        always {
            script {
                notify("Himateja0206@gmail.com")
            }
            cleanWs()
        }
    }
}
