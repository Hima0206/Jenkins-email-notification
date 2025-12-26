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
                body: """<html>
                        <body>
                        <div style="background-color:#4CAF50; color:white; padding:10px; text-align:center;">
                            <h2> Jenkins Build Notification</h2>
                        </div>

                        <p>Status: <b style="color:${currentBuild.currentResult == 'SUCCESS' ? 'green' : 'red'}">
                            ${currentBuild.currentResult}
                        </b></p>
                        <p><b>Build Job:</b> ${env.JOB_NAME}</p>
                        <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                        <p><b>Build URL:</b> ${env.BUILD_URL}</p>
                        <p><b>Branch Name:</b> ${env.GIT_BRANCH}</p>
                        <p><b>Triggered by:</b> ${currentBuild.getBuildCauses()[0].shortDescription}</p>
                        <p><b>Duration:</b> ${currentBuild.durationString}</p>
                        <p><b>Commit hash:</b> ${env.GIT_COMMIT}</p>
                        </body>
                        </html>""",
                to: 'Himateja0206@gmail.com',
                mimeType: 'text/html'
            )

            cleanWs()
        }
    }
}
