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
                            <h3 style="color:blue;">Jenkins Build Notification</h3>
                            <p><b>Project:</b> ${env.JOB_NAME}</p>
                            <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                            <p><b>Build URL:</b> ${env.BUILD_URL}</p>
                            <p><b>Branch Name:</b> ${env.GIT_BRANCH}</p>
                            <p><b style="color:${currentBuild.currentResult == 'SUCCESS' ? 'green' : 'red'};">
                                Status: ${currentBuild.currentResult}
                            </b></p>
                            <p><b>Triggered by:</b> ${currentBuild.getBuildCauses()[0].shortDescription}</p>
                            <p><b>Duration:</b> ${currentBuild.durationString}</p>
                            <p><b>Commit hash:</b> ${env.GIT_COMMIT}</p>
                            <p>Check console output at: 
                                <a href="${env.BUILD_URL}">${env.BUILD_URL}</a>
                            </p>
                        </body>
                        </html>""",
                to: 'Himateja0206@gmail.com',
                mimeType: 'text/html'
            )
        }

        cleanup {
            cleanWs()
        }
    }
}
