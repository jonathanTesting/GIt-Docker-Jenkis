def CURRENT_STAGE = 'Started'
def COMMITER = 'manually'

pipeline {
    agent {
        docker { image 'node:14-alpine' } 
    }

         stage("Initialize"){
            steps{
                script{
                    CURRENT_STAGE = 'Initialize'
                }
                container("node") {
                    sh "node -v"
                    sh "npm cache clean --force"
                    sh "npm install"
                    //sh "npm update"
                    sh "yarn install"
                    sh "yarn upgrade"
                }
            }
        }

    }
	