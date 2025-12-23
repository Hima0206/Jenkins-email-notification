@Library('Jenkins-shared-library') _
pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                sh "echo 'Checkout Completed'"
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
        }
    }
}
