pipeline {
  environment {
    DOCKERHUB_REPOSITORY = "kyontoki/nginx"
    DOCKERHUB_CREDENTIALS = credentials('kyontoki')
    dockerImage = ''
  }

  
  stages {
    
    stage('Checkout Application Git Branch') {
        steps {
            git credentialsId: 'kyontoki',
                url: 'https://github.com/JaeBumPark/CICD.git', /* URL변경에 따른 수정 필요 */
                branch: 'main'
        }
        post {
                failure {
                  echo 'Repository clone failure !'
                }
                success {
                  echo 'Repository clone success !'
                }
        }
    }.  
  

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
        docker build -t ${dockerHubRegistry}:${currentBuild.number}"
        docker build -t ${dockerHubRegistry}:latest"
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
