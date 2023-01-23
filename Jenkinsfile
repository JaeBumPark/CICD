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
        docker build -t ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}"
        docker build -t ${ DOCKERHUB_REPOSITORY}:latest"
        '''
      }
    }
    
    stage('deploy deploy') {
      steps {
        sh '''
        docker tag nginx:latest $DOCKERHUB_REPOSITORY:1.0
        docker push $DOCKERHUB_REPOSITORY:1.0
        '''
      }
    }
  }
}
