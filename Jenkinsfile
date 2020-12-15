def CURRENT_STAGE = 'Started'
def COMMITER = 'manually'
def artifactTag = "https://02pelevacontregd01.azurecr.io"
def artifactVersion


pipeline {
    agent {
        kubernetes {
            defaultContainer "jnlp"
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: ci
spec:
  volumes:
  - name: dockersock
    hostPath:
      path: "/var/run/docker.sock"
  - name: docker
    hostPath:
      path: "/usr/bin/docker"
  - name: service-principal
    secret:
      secretName: eleva-conection-conf-dev
  serviceAccount: cd-jenkins
  containers:
  - name: azure
    image: mcr.microsoft.com/azure-cli
    volumeMounts:
    - name: docker
      mountPath: "/usr/bin/docker"
    - name: dockersock
      mountPath: "/var/run/docker.sock"
    command:
    - cat
    tty: true
  - name: docker
    image: docker:19.03
    volumeMounts:
    - name: docker
      mountPath: "/usr/bin/docker"
    - name: dockersock
      mountPath: "/var/run/docker.sock"
    command:
    - cat
    tty: true   

"""
        }
    }
	environment {
		TEAMS_HOOK = 'https://outlook.office.com/webhook/bd615747-68e3-477d-994c-94e6ba276b48@1137d507-c7ac-474a-bc77-3f11dda69860/JenkinsCI/f12c9feb768743538ece10a87a1a31b6/c46aec4b-eaca-44dd-b652-8a4112f7d3c4'
	}
    stages {
        stage ('Solicitar versión a desplegar') {

      }
    }

       stage('Git TAG Generation'){
      steps{
        withCredentials([usernamePassword(credentialsId: 'aduque-gitlab-davivienda', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
        // some block
          sh 'git config user.email "aduque@dataifx.com"'
          sh "git tag -a v0.0.${artifactVersion} -m 'version 0.0.${artifactVersion}'"
          sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@git.co.davivienda.com/bancapatrimonial/eleva/eleva-aspnet.git --tags'
        }
      }
    }
        stage('Deploy Ambiente LAB') {
            steps {
                script{
                    CURRENT_STAGE = 'Deploy Ambiente LAB'
                }
                container("docker") {
                    withCredentials([usernamePassword(credentialsId: 'jenkins-azure-service-account', usernameVariable: "username", passwordVariable: "password")]) {
				        sh "docker login 02pelevacontregd01.azurecr.io -u $username -p $password"
				        sh "docker pull 02pelevacontregd01.azurecr.io/dataifxelevaweb-${artifactVersion}:${artifactVersion}"
				        sh "docker tag dataifxelevaweb-${artifactVersion} 02PELEVACONTREGL01.azurecr.io/dataifxelevaweb-${artifactVersion}:${artifactVersion}"
				        sh "docker push 02PELEVACONTREGL01.azurecr.io/dataifxelevaweb-${artifactVersion}:${artifactVersion}"
                    }
                }
                container("azure") {
                    withCredentials([usernamePassword(credentialsId: 'jenkins-azure-dev', usernameVariable: "username", passwordVariable: "password")]) {
                        sh "az login  -u $username -p $password"
                        sh "az webapp config container set --name 02P-ELEVAAPI-L01 --resource-group RG-ELEVA-L --docker-custom-image-name 02PELEVACONTREGL01.azurecr.io/dataifxelevaweb-${artifactVersion}:${artifactVersion} --docker-registry-server-url https://02PELEVACONTREGL01.azurecr.io"

                    }
                }
            }
        }
    }
	post {
        aborted {
            office365ConnectorSend color: '#af002a', message: 'The Pipeline with ID ' + env.BUILD_DISPLAY_NAME + ' was Aborted on stage ' + CURRENT_STAGE, webhookUrl: env.TEAMS_HOOK
        }
        failure {
        office365ConnectorSend color: '#af002a', message: 'The Pipeline with ID ' + env.BUILD_DISPLAY_NAME + ' Failed on stage ' + CURRENT_STAGE, webhookUrl: env.TEAMS_HOOK
        }
        success {
            script{
                CURRENT_STAGE = "Success"
            }
            office365ConnectorSend color: '#00cc00', message: 'The Pipeline with ID ' + env.BUILD_DISPLAY_NAME + ' was executed Succesfully!', webhookUrl: env.TEAMS_HOOK
        }
    }
}