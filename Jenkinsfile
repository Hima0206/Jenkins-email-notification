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
                        <div style="background-color:${(currentBuild.currentResult == 'SUCCESS') ? 'green' : 'red'}; color:white; padding:10px; text-align:center;">
                            <h2>Jenkins Build Notification</h2>
                        </div>
                        <table border="1" cellspacing="0" cellpadding="5" style="border-collapse:collapse; width:100%;">
                        <tr><th align="left">Status</th><td style="color:${(currentBuild.currentResult == 'SUCCESS') ? 'green' : 'red'}">${currentBuild.currentResult}</td></tr>
                        <tr><th align="left">Build Job</th><td>${env.JOB_NAME}</td></tr>
                        <tr><th align="left">Build Number</th><td>${env.BUILD_NUMBER}</td></tr>
                        <tr><th align="left">Build URL</th><td><a href="${env.BUILD_URL}">${env.BUILD_URL}</a></td></tr>
                        <tr><th align="left">Branch Name</th><td>${env.GIT_BRANCH}</td></tr>
                        <tr><th align="left">Triggered by</th><td>${currentBuild.getBuildCauses()[0].shortDescription}</td></tr>
                        <tr><th align="left">Duration</th><td>${currentBuild.durationString}</td></tr>
                        <tr><th align="left">Commit hash</th><td>${env.GIT_COMMIT}</td></tr>
                        </table>
                        </body>
                        </html>""",
                to: 'Himateja0206@gmail.com',
                mimeType: 'text/html'
            )

            cleanWs()
        }
    }
}
