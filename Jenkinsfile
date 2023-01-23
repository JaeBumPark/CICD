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
        docker build -t ${DOCKERHUB_REPOSITORY}:latest"
        '''
      }
    }
     stage('Docker Image Push') {
       steps {
         sh '''     
         docker push ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}"
         docker push ${DOCKERHUB_REPOSITORY}:latest"
         sleep 10 /* Wait uploading */
         '''         
            }
            post {
                    failure {
                      echo 'Docker Image Push failure !'
                      sh "docker rmi ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}"
                      sh "docker rmi ${DOCKERHUB_REPOSITORY}:latest"
                    }
                    success {
                      echo 'Docker image push success !'
                      sh "docker rmi ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}"
                      sh "docker rmi ${DOCKERHUB_REPOSITORY}:latest"
                    }
            }
    }
  }
}
