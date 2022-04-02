pipeline {

  environment {
    REGISTRY_NAME = "st251/web_server"
    registryCredential = 'dockerhub-st251-jenkins'
    dockerImage = ''
  }
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  
stages { 
    stage('Building our image') {
            steps {
                script {
                    app = docker.build("st251/web_server")
                }  
            }
        }
   stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-st251-jenkins') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
   stage('docker run') {
            steps {
                  sh 'docker run --name web_server -p 8282:8282'
            }
        }
 
   stage('HealthCheck') {
      steps {
             
             sleep 300
             sh 'curl http://localhost:8282'
      }
   }
   stage('Remove Unused docker image') {
      steps{
        input 'Remove Unused docker image?'
        sh "docker rmi ${REGISTRY_NAME}"
      }
    }
  }
   
}
