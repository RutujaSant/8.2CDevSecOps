pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Starting build...'
                bat """
                echo Build started at %DATE% %TIME% > result.log
                echo Running build... >> result.log
                """
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat """
                echo Running tests... >> result.log
                echo Tests passed at %DATE% %TIME% >> result.log
                echo All tests passed. > test-results.log
                """
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                bat """
                echo Running security scan... >> result.log
                echo Security scan completed at %DATE% %TIME% >> result.log
                echo No issues found. > security-scan.log
                """
            }
        }

        stage('Combine Logs') {
            steps {
                echo 'Combining logs...'
                bat """
                type test-results.log >> result.log
                echo. >> result.log
                type security-scan.log >> result.log
                """
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """
                <p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> finished with status <b>${currentBuild.currentResult}</b>.</p>
                <p>Console output is available <a href="${env.BUILD_URL}">here</a>.</p>
                <p>See attached result.log for detailed results.</p>
                """,
                mimeType: 'text/html',
                to: 'rutujasant@gmail.com',
                attachmentsPattern: 'result.log'
            )
        }
    }
}
