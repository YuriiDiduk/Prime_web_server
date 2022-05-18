pipeline {
  agent any
     stages{

        stage('Deployx'){
          steps {
             script {
                 withKubeConfig([credentialsId: 'config', serverUrl: '']) {
                   sh 'kubectl apply -f web_deploy.yml'
                        }
                    }
                }
            }
        }
    }
