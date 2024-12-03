pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    REGISTRY = "atrium5365"
    DOCKERHUB_CREDENTIALS = credentials('atrium5365-dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t ${REGISTRY}/dp-alpine:latest .'
      }
    }
    stage('Debug') {
      steps {
        sh 'echo Username: $DOCKERHUB_CREDENTIALS_USR'
        sh 'echo Password: ${DOCKERHUB_CREDENTIALS_PSW:0:4}****'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push ${REGISTRY}/dp-alpine:latest'
      }
    }
  }
  post {
    always {
      sh 'docker rmi ${REGISTRY}/dp-alpine:latest || true'
      sh 'docker logout'
    }
  }
}