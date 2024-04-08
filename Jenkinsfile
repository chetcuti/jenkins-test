pipeline {
    agent any

   environment {
       DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-test')
   }

   stages {

     stage('git Checkout') {
         steps {
             git branch: 'main', url: 'https://github.com/chetcuti/jenkins-test.git'
         }
     }

     stage('Docker Login') {
         steps {
             echo 'Logon in to docker hub'
             sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin docker.io'
             echo 'Login Successfull'
         }
     }

     stage('Build Docker Image') {
         steps {
             script {
                 app = docker.build("gjch/jenkins-test:${env.BUILD_NUMBER}")
             }
         }
     }

     stage('Push Docker Image') {
         steps {
             script {
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials-test') {
                     app.push("${env.BUILD_NUMBER}")
                     app.push("latest")
                 }
             }
         }
     }

   }

   post {
       success {
           echo 'Docker image built and pushed successfully!'
       }
   }
}
