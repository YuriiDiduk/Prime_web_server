pipeline{
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
        
        stage('Compile'){
            when {
                expression { params.TEST == true }
            }
            steps {
         sh 'mvn compile' //only compilation of the code
       }
    }
        stage('SonarQube'){
            when {
                expression { params.TEST == true }
            }
       steps {
       withSonarQubeEnv(installationName: 'sq2') {
        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
        }
      }
       steps {
  timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
}
       steps {       
            sh 'docker run --rm -i hadolint/hadolint < Dockerfile1' 
             }
     }
             
        stage('Build and push image to DHub'){
            when {
                expression { params.BUILD == true }
            }
            steps {
                script {
                    dockerImage = docker.build "st251/petclinicx:$BUILD_NUMBER"
                    dockerImage = docker.build "st251/petclinicx:latest"
                }
            }
            steps {
                script {
                    // Assume the Docker Hub registry by passing an empty string as the first parameter
                    docker.withRegistry('' , 'dockerhub-cred-st251') {
                        dockerImage.push()
                    }
                }
            }
        }
     
        stage('Deploy'){
            when {
                expression { params.DEPLOY == true }
            }
            steps{
             input 'Deploy to Production?'
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    script {
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker pull willbla/train-schedule:${env.BUILD_NUMBER}\""
                        try {
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stop train-schedule\""
                            sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker rm train-schedule\""
                        } catch (err) {
                            echo: 'caught error: $err'
                        }
                        sh "sshpass -p '$USERPASS' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker run --restart always --name train-schedule -p 8080:8080 -d willbla/train-schedule:${env.BUILD_NUMBER}\""
                    }
                }
            }
        }
    }
}
