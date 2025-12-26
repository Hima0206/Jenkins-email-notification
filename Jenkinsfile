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
            script {
                def jobName = env.JOB_NAME
                def buildNumber = env.BUILD_NUMBER
                def buildStatus = currentBuild.currentResult
                def buildUrl = env.BUILD_URL

                def triggeredBy = currentBuild
                    .getBuildCauses('hudson.model.Cause$UserIdCause')[0]
                    ?.getUserName() ?: 'N/A'

                def buildDuration = currentBuild.durationString.replace(' and counting', '')
                def branchName = env.GIT_BRANCH ?: 'N/A'

                def lastCommitHash = sh(
                    script: "git rev-parse HEAD || echo N/A",
                    returnStdout: true
                ).trim()

                def bannerColor =
                    buildStatus.toUpperCase() == 'SUCCESS' ? 'green' :
                    buildStatus.toUpperCase() == 'FAILURE' ? 'red' : 'orange'

                def bodyContent = """
                <h2 style="color:${bannerColor};">Build Status: ${buildStatus}</h2>
                <ul>
                    <li><strong>Job Name:</strong> ${jobName}</li>
                    <li><strong>Build Number:</strong> ${buildNumber}</li>
                    <li><strong>Branch Name:</strong> ${branchName}</li>
                    <li><strong>Last Commit Hash:</strong> ${lastCommitHash}</li>
                    <li><strong>Triggered By:</strong> ${triggeredBy}</li>
                    <li><strong>Build Duration:</strong> ${buildDuration}</li>
                    <li><strong>Build URL:</strong>
                        <a href="${buildUrl}">${buildUrl}</a>
                    </li>
                </ul>
                """

                emailext(
                    subject: "Build Notification: ${jobName} #${buildNumber} - ${buildStatus}",
                    body: bodyContent,
                    to: 'himateja0206@gmail.com',
                    mimeType: 'text/html'
                )
            }

            cleanWs()
        }
    }
}
