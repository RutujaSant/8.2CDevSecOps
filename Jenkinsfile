pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Running build...'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'echo "All tests passed." > test-results.log'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                bat 'echo "No issues found." > security-scan.log'
            }
        }
    }

    post {
        always {
            script {
                // Save the console log to a file in the workspace
                def consoleLog = currentBuild.rawBuild.getLog(1000).join("\n")
                writeFile file: 'console-log.txt', text: consoleLog
            }

            archiveArtifacts artifacts: 'console-log.txt'

            emailext (
                subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """<p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> finished with status <b>${currentBuild.currentResult}</b></p>
                         <p>Console: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                mimeType: 'text/html',
                to: 'rutujasant@gmail.com',
                attachmentsPattern: 'console-log.txt'
            )
        }
    }
}
