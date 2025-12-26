pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                echo 'Checkout Completed'
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
                echo 'Build Completed'
            }
        }
    }

    post {
        always {
            emailext(
                subject: "Pipeline status: ${currentBuild.currentResult}",
                body: '''<html>
                        <body>
                            <h3>Jenkins Build Notification</h3>
                            <p>Project: ${env.JOB_NAME}</p>
                            <p>Build Number: ${env.BUILD_NUMBER}</p>
                            <p>Build URL: ${env.BUILD_URL}</p>
                            <p>Branch Name: ${env.GIT_BRANCH}</p>
                            <p>Status: ${currentBuild.currentResult}</p>
                            <p>Triggered by: ${currentBuild.getBuildCauses()[0].shortDescription}</p>
                            <p>Duration: ${currentBuild.durationString}</p>
                            <p>Commit hash: ${env.GIT_COMMIT}</p>
                            <p>Check console output at: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                        </body>
                        </html>''',
                to: 'Himateja0206@gmail.com',
                mimeType: 'text/html'
            )
        }

        cleanup {
            cleanWs()
        }
    }
}
