pipeline {
  agent any
     stages{

        stage('Deployx'){
          steps {
                 withKubeConfig([credentialsId: 'config', serverUrl: 'https://0ADC4FD916ACB6CB1B1D03BFDF743B73.gr7.us-east-1.eks.amazonaws.com']) {
                   sh 'kubectl apply -f ./web_deploy.yml'
                        }
                  }
            }
        }
    }
