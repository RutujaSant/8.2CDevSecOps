pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                echo 'Starting compilation...'
                // simulate compile step
            }
        }

        stage('Unit Tests') {
            steps {
                echo 'Executing unit tests...'
                bat 'echo Test suite completed successfully > unit-test-report.txt'
            }
        }

        stage('Vulnerability Assessment') {
            steps {
                echo 'Performing vulnerability assessment...'
                bat 'echo No vulnerabilities detected > vulnerability-report.txt'
            }
        }
    }

    post {
        always {
            script {
                def buildStatus = currentBuild.currentResult ?: 'UNKNOWN'
                def jobName = env.JOB_NAME ?: 'Unknown Job'
                def buildNumber = env.BUILD_NUMBER ?: 'Unknown Build'
                def buildUrl = env.BUILD_URL ?: '#'

                emailext(
                    subject: "Notification: ${jobName} Build #${buildNumber} - ${buildStatus}",
                    body: """
                        <h3>Build Report</h3>
                        <p>Project: <b>${jobName}</b></p>
                        <p>Build Number: <b>${buildNumber}</b></p>
                        <p>Status: <span style="color:${buildStatus == 'SUCCESS' ? 'green' : 'red'}"><b>${buildStatus}</b></span></p>
                        <p>Check the <a href="${buildUrl}">build logs</a> for details.</p>
                    """,
                    mimeType: 'text/html',
                    to: 'rutujasant@gmail.com'
                )
            }
        }
    }
}
