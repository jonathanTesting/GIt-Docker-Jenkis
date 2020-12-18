pipeline {
    agent any

    stages {
        steps {
				script{
                    CURRENT_STAGE = 'Build'
                }
                container("dotnet") {
					sh "dotnet build"
                }
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