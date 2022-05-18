pipeline {
  agent any
     stages{

        stage('Deployx'){
          steps {
                 withKubeConfig([credentialsId: 'config', serverUrl: '']) {
                   sh 'kubectl apply -f web_deploy.yml'
                        }
                  }
            }
        }
    }
