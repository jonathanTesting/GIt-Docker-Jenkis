pipeline {
    agent any

    stages {

        stage('Build') {
            
            steps {
				script{
                    CURRENT_STAGE = 'Build'
                }
                container("dotnet") {
                    sh "ls"
					sh "dotnet build"
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