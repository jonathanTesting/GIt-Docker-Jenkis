pipeline {
    agent {docker { image 'node'}

    }
    stages {
        stage('Git [develop]'){
            steps{
                script{
                    CURRENT_STAGE = 'Git [develop]'
                }
                git branch: 'develop', credentialsId: 'aduque-gitlab-davivienda', url: 'https://git.co.davivienda.com/bancapatrimonial/eleva/eleva-angular.git'
                script {
                    COMMITTER_NAME = sh (
                        returnStdout: true,
                        script: 'git --no-pager show -s --format=\'%an\''
                    ).trim()
                    
                     if(COMMITTER_NAME != null){
                        COMMITER = 'by ' + COMMITTER_NAME
                    } 
                }
            }
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
        stage('Deploy Dev.') {
            steps {
                script{
                    CURRENT_STAGE = 'Deploy Dev.'
                }
                // dir('dist') {
                //     container("azure") {
                //         sh "az login --service-principal -u http://sp-dev-lab-jenkins -p pwIo2o_udhrITY70S0DD~RnKnVI.wNY05J --tenant d36775fa-f481-4695-86f4-41432c8f57af"
                        
                //         sh "az storage blob delete-batch --source '\$web' --account-name 02pelevastoraged --auth-mode login"
                //         sh "az storage blob upload-batch -d '\$web' -s 'apps/admin' --account-name 02pelevastoraged --auth-mode login"
                        
                //         sh "az storage blob delete-batch --source '\$web' --account-name 03elevastoraged --auth-mode login"
                //         sh "az storage blob upload-batch --source 'apps/app-mobile' --destination '\$web' --account-name 03elevastoraged --auth-mode login"
                        
                //         sh "az storage blob delete-batch --source '\$web' --account-name 06pelevastoraged --auth-mode login"
                //         sh "az storage blob upload-batch --source 'apps/container-mock' --destination '\$web' --account-name 06pelevastoraged --auth-mode login"
                        
                //         sh "az storage blob delete-batch --source '\$web' --account-name 07pelevastoraged --auth-mode login"
                //         sh "az storage blob upload-batch --source 'apps/app-mobile-two' --destination '\$web' --account-name 07pelevastoraged --auth-mode login"
                    // }
                }
            }
        }
    }
    
    }
}
