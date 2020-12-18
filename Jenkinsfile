pipeline {
    agent {
    }
    stages {
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
        stage('Build') {
            steps {
                script{
                    CURRENT_STAGE = 'Build'
                }
                container("node") {
                    sh "npm run buildElevaAdmin"
                    sh "npm run buildElevaMobile"
                    sh "npm run buildContainerDEV"
                    sh "npm run buildElevaMobileTwo"
                }
            }
        }
        stage('Tests') {
            steps {
                script{
                    CURRENT_STAGE = 'Tests'
                }
            }
        }
        
    }
}
