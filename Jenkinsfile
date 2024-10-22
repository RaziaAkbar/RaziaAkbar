pipeline {
    agent any

    parameters {
        string(name: 'ENV', defaultValue: 'dev', description: 'Environment to deploy to')
    }

    environment {
        MVN_HOME = tool name: 'Maven 3', type: 'hudson.tasks.Maven$MavenInstallation' // Assumes Maven is configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                checkout scm // Checkout source code from version control
            }
        }

        stage('Build') {
            steps {
                echo "Building project..."
                sh "'${MVN_HOME}/bin/mvn' clean package" // Adjust Maven build command if needed
            }
        }

        stage('Unit Tests') {
            steps {
                echo "Running unit tests..."
                sh "'${MVN_HOME}/bin/mvn' test" // Execute unit tests
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis..."
                withSonarQubeEnv('SonarQube') { // Ensure SonarQube is configured in Jenkins
                    sh "'${MVN_HOME}/bin/mvn' sonar:sonar" // SonarQube analysis
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true // Wait for SonarQube quality gate result
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                expression {
                    return params.ENV == 'prod' || params.ENV == 'dev' // Deploy only for 'dev' or 'prod' environments
                }
            }
            steps {
                echo "Deploying application to ${params.ENV} environment..."
                // Add your deployment commands here
                // sh 'deployment-script.sh' // Example placeholder for deployment command
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

        always {
            node('maven-build') { // Added label for the Maven Build node
                cleanWs() // Clean up workspace after build
            }
        }
    }
}
