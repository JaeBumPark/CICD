pipeline {
    agent any
  environment {
    DOCKERHUB_REPOSITORY = "kyontoki/nginx"
    DOCKERHUB_CREDENTIALS = credentials('kyontoki')
    dockerImage = ''
  }

  
  stages {
    
    stage('git scm update') {
      steps {
        git url: 'https://github.com/JaeBumPark/CICD.git', branch: 'main'
      }
    }
    stage('docker login') {
      steps {
        sh '''
        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
        '''
      }
    }
    stage('docker build') {
      steps {
        sh '''
        docker build . -t ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}
        '''
      }
    }
     stage('Docker Image Push') {
       steps {
         sh '''     
         docker push ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}"
         
         sleep 10 /* Wait uploading */
         '''         
            }     
    }
  }
}
