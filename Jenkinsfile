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
    tools {
        // Install the SonarQube Scanner
        sonarQube 'SonarScanner'
    }
    environment {
        // Set the environment variable for SonarCloud analysis
        SONAR_TOKEN = credentials('your-sonarcloud-token-id')
    }
    stages {
        stage('Build') {
            steps {
                // Example build steps
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('SonarCloud') {
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=<your_project_key> \
                            -Dsonar.organization=<your_organization> \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=${env.SONAR_TOKEN}"
                    }
                }
            }
        }
    }
}
