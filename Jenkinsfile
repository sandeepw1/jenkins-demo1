pipeline {
  agent any 
  environment {
    DOCKER_CRED = credentials('DockerHub')
  }
  stages {
    stage('fetch-code') {
      steps {
        git branch: 'main', url: 'https://github.com/sandeepw1/jenkins-demo1.git'
      }
    }
    stage('build-image') {
      steps {
        sh 'docker build -t sandeepwalvekar/jenkins-app:${BUILD_NUMBER} .'
      }
    }
    stage('push-image') {
      steps {
        sh 'echo $DOCKER_CRED_PSW | docker login -u $DOCKER_CRED_USR --password-stdin'
        sh 'docker push sandeepwalvekar/jenkins-app:${BUILD_NUMBER}'
      }
    }
    stage('update-minikube') {
      steps {
        sh 'kubectl set image deployment/pd1 jenkins-app=sandeepwalvekar/jenkins-app:${BUILD_NUMBER}'
      }
    }
  }
}
