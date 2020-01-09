pipeline {
  environment {
    dockerRegistry = "kkajnabi/docker-nodejs"
    dockerRegistryCredential = 'docker-hub'
    dockerImage = ''
  }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/kumarkundan/node-hello-world.git'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
   
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build dockerRegistry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Upload Image') {
      steps{
        script {
          docker.withRegistry( '', dockerRegistryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
     stage('Run') {
       steps {
         script {
          dockerImage = docker.run dockerRegistry + ":$BUILD_NUMBER"
        }
       }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $dockerRegistry:$BUILD_NUMBER"
      }
    }
  }
}
