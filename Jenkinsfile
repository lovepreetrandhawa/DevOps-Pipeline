pipeline {
    agent any

    tools {
        nodejs "NodeJS" // Assuming "NodeJS" is configured globally in Jenkins
        git "Default"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    bat 'npm install'
                }
            }
        }

        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        script {
                            bat 'npm test'
                        }
                    }
                }
                stage('Test with Coverage') {
                    steps {
                        script {
                            bat 'npm test -- --coverage'
                        }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'npm run build'
                }
            }
        }

        stage('Deploy') {
    steps {
        script {
            // Start the server in the background
            bat 'start npm run start -- -p 3000'
            // Add a short delay to allow the server to start before moving to the next step
            bat 'ping 127.0.0.1 -n 11 >NUL'
        }
    }
    post {
        failure {
            echo 'Deployment failed!'
            // Add cleanup tasks if necessary
            // For example, rollback changes, stop services, etc.
        }
    }
}

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}
