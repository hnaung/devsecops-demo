pipeline {
  environment {
    registry = "hnaung/docker-test"
    registryCredential = ‘dockerhub_id’
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/hnaung/devsecops-demo.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
  }
}
