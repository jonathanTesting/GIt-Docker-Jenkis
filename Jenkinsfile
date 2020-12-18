pipeline {
    agent any

    stages {

        stage('Build') {
                        steps {
				script{
                    CURRENT_STAGE = 'Build'
                }
                container("dotnet") {
					sh "dotnet restore"
                }
            }

        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}