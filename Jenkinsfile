pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                script {
                    // Restoring dependencies
                    //bat "cd ${DOTNET_CLI_HOME} && dotnet restore"
                    sh "dotnet restore"

                    sh "dotnet clean"

                    // Building the application
                    sh "dotnet build --configuration Release"
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    // Publishing the application
                    sh "dotnet publish --no-restore --configuration Release --output .\\publish"
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sshagent(['SSHcred'])
                    {
                    sh "scp -r -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Dotnetproject/.publish/* ubuntu@52.201.146.173:/var/www/app/"
                    
                    }
                }
            }
        }

        stage('Service restart') {
            steps {
                script {
                    sshagent(['SSHcred']) {
                        sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@52.201.146.173 << EOF
                        sudo systemctl restart kestrel-app.service
                        EOF
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment is successful!'
        }
    }
}
