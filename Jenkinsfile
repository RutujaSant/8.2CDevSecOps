post {
    success {
        echo 'Build succeeded - no email sent.'
    }
    failure {
        script {
            def consoleLog = currentBuild.rawBuild.getLog(1000).join("\n")
            writeFile file: 'console-log.txt', text: consoleLog
        }
        archiveArtifacts artifacts: 'console-log.txt'
        emailext (
            subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - FAILED",
            body: """<p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> failed.</p>
                     <p>Console: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
            mimeType: 'text/html',
            to: 'rutujasant@gmail.com',
            attachmentsPattern: 'console-log.txt'
        )
    }
}
