pipeline {
  environment {
    registry = "milan"
    registryCredential = 'awscred'
    dockerImage = ''
    ECRURL = 'https://780862318210.dkr.ecr.ap-south-1.amazonaws.com/milan'
    ECRCRED = 'ecr:ap-south-1:awscred'
  }
  agent any
  stages {
    stage('Get SCM') {
      steps {
        sh 'rm -rf ReactJsApp' 
        sh 'git clone https://github.com/DPMadhavan/ReactJsApp.git'
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
          //docker.withRegistry( '', registryCredential ) {
            docker.withRegistry(ECRURL,ECRCRED) {
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
