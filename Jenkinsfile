pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/username/project.git'
        SONAR_PROJECT_KEY = 'your-sonar-project-key'
        SONAR_HOST_URL = 'https://sonarcloud.io'
        SONAR_LOGIN = credentials('SONAR_TOKEN_ID')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: "${GIT_REPO_URL}", branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'npm audit --json > audit-report.json || true'
                // Continue even if vulnerabilities are found
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_LOGIN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            echo "Build finished with status: ${currentBuild.result}"
        }

        success {
            echo "✅ Build succeeded!"
        }

        failure {
            echo "❌ Build failed."
        }

        unstable {
            echo "⚠️ Build is unstable (e.g., test failures or low code coverage)."
        }
    }
}
