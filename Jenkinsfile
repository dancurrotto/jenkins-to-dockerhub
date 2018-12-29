pipeline {
  environment {
    registry = "dcurrotto/dotnet-core-simple"
    registryCredential = 'DockerHubCredentials'
    dockerImage = ''
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/dancurrotto/jenkins-to-dockerhub.git'
      }
    }
    stage('Build') {
       steps {
         sh 'echo building'
       }
    }
    stage('Test') {
      steps {
        sh 'echo testing'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}