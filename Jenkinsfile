pipeline {
  agent any
  environment {
   DOCKERHUB_CREDENTIALS = credentials('docker-cred') 
  }
  stages {
    stage('Checkout') {
      steps {
        git branch: 'master', url: 'https://github.com/ijaz0059/httpbin.git'
      }
    }
    
    stage('Build Docker Image') {
      steps {
       sh 'docker build -t $DOCKERHUB_CREDENTIALS_USR/httpbin:v$BUILD_NUMBER .' 
      }
    }
    
    stage('Login to DockerHub') {
      steps {
       sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
       }
    }
    
    stage('Push Docker Image to DockerHub') {
      steps {
        sh 'docker push $DOCKERHUB_CREDENTIALS_USR/httpbin:v$BUILD_NUMBER'
      }
    }
    stage('Run docker image') {
       steps {  
         sh 'docker run -it -dp 81:80 $DOCKERHUB_CREDENTIALS_USR/httpbin:v$BUILD_NUMBER' // Optional
       }
    }
  }
  post {
    always {
       sh 'docker logout'
    }
  }
}

  
