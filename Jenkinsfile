pipeline {

  environment {
     commit = 'csmsmcmdvmsdvmd'
  }
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  
stages { 
    stage('Building our image') {
            steps {
                script {
                  echo "dfjkdfk ${commit}"
                }  
            }
        }
   stage('Push Docker Image') {
            steps {
                script {
                    echo "this is ${commit}"
                    }
                }
            }
        }
   stage('docker run') {
            steps {
                  echo "this is ${env.BUILD_NUMBER} on ${env.JENKINS_URL}"
            }
        }
 
   stage('Remove Unused docker image') {
      steps{
        input 'Remove Unused docker image?'
        echo "this is ${env.GIT_COMMIT}"
      }
    }
  }
   
}
