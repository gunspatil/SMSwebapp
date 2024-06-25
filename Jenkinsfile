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
                    sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/Dotnetproject/smswebapp/bin/Release/net8.0/* ubuntu@54.80.249.214:/var/www/app/"
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
