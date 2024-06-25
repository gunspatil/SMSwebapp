pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                script {
                    // Restoring dependencies
                    //bat "cd ${DOTNET_CLI_HOME} && dotnet restore"
                    sh "dotnet restore"

                    // Building the application
                    sh "dotnet build --configuration Release"
                }
            }
        }


        stage('Deploy') {
            steps {
                script {
                    // Publishing the application
                    sh "sudo scp -i .ssh/id_ed25519 /var/lib/jenkins/workspace/SMSFreestyle/smswebapp/bin/Debug/net8.0/* ubuntu@54.80.249.214:/var/www/app/"
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
