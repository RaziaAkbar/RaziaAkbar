pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Build your project (compile, package, etc.)
                sh 'mvn clean package'
            }
        }

        stage('Upload Artifacts') {
            steps {
                // Define the Artifactory server and repository
                rtUpload(
                    serverId: 'JFrog-Server',
                    spec: '''{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "my-repo/"
                            }
                        ]
                    }'''
                )
            }
        }
    }
}
