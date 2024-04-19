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
                    bat 'npm start -- -p 3000'
                }
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
