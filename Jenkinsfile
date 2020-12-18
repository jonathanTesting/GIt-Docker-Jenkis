pipeline {
    agent any

    stages {
         stage('main'){
            steps{
				script{
                    CURRENT_STAGE = 'main'
                }
                git branch: 'main', credentialsId: 'jonathanTesting', url: 'https://github.com/jonathanTesting/GIt-Docker-Jenkis.git'
                
                script {
                    COMMITTER_NAME = sh (
                        returnStdout: true,
                        script: 'git --no-pager show -s --format=\'%an\''
                    ).trim()
                    
                     if(COMMITTER_NAME != null){
                        COMMITER = 'by ' + COMMITTER_NAME
                    } 
                }
                
                office365ConnectorSend message: 'The Pipeline with ID ' + env.BUILD_DISPLAY_NAME + ' was started ' + COMMITER, webhookUrl: env.TEAMS_HOOK
            }
        }

        stage('Build') {
                        steps {
				script{
                    CURRENT_STAGE = 'Build'
                }
                container("dotnet") {
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