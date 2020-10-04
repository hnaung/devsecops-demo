pipeline {
  environment {
    registry = "hnaung/node-app"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/hnaung/devsecops-demo'
      }
    }
//    stage('Build') {
//       steps {
//        sh 'npm install'
//       }
//    }
//    stage('Test') {
//      steps {
//        sh 'npm test'
//      }
//    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Scan') {
      steps{
        aquaMicroscanner imageName: 'hnaung/node-app:"$BUILD_NUMBER"', notCompliesCmd: 'echo "The Aqua has failed', onDisallowed: 'fail', outputFormat: 'html'
        }
      }
//    stage('Analyze with Anchore plugin') {
//      steps {
//        def imageLine = 'openjdk:8-jre-alpine'
//        writeFile file: 'anchore_images', text: imageLine
//        anchore name: 'anchore_images'
//      }
//    }

    stage('Publish Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
