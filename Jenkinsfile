pipeline {
  agent any
     stages{

        stage('Deployx'){
          steps {
             script {
                 withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'config4', namespace: '', serverUrl: '') {
                     sh 'kubectl apply -f web_deploy.yml'
                             } 
                   
                        }
                    }
                }
            }
        }
    
