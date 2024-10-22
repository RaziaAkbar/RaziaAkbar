pipeline {
    agent any 
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package' // Adjust based on your build command
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test' // Adjust based on your test command
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh 'mvn sonar:sonar' // Adjust based on your analysis command
                }
            }
        }

        stage('Deploy') {
            steps {
                // Add deployment commands here if applicable
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

