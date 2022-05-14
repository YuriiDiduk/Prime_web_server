pipeline {
  agent any
    parameters {
        booleanParam (
            defaultValue: true,
            description: 'Compile and testing SonarQube',
            name: 'TEST')
        booleanParam (
            defaultValue: true,
            description: 'Build and push images to DockerHub',
            name: 'BUILD')
        booleanParam (
            defaultValue: true,
            description: 'Deploy to K8S',
            name: 'DEPLOY')
    }
     stages{
       stage('Test'){
            when {
                expression { params.TEST == true }
            }
            steps {
                echo "Now we begin SMOKE TEST"
            }
        }

       stage('D-Build'){
            when {
                expression { params.BUILD == true }
               }
            steps {
                echo "Now we begin BUILD"
                sh "docker build -t st251/web_server:${env.BUILD_NUMBER} ."
                 }
              }
       stage('D-push') {
            when {
                expression { params.BUILD == true }
               }
           steps {
                echo "Now we begin push"
                withDockerRegistry(credentialsId: 'Dhub-cres', url: 'https://index.docker.io/v1/'){
                    sh "docker push st251/web_server:${env.BUILD_NUMBER}"
                    sleep 60
                    sh "docker rmi st251/web_server:${env.BUILD_NUMBER}"           
                 } 
             }
           }
        stage('Deploy'){
            when {
                expression { params.DEPLOY == true }
            }
            steps{
             input 'Deploy to Production?'       
                
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'web_deploy.yml',
                    enableConfigSubstitution: true
                 )
               
              } 
            }
        }
    }
