pipeline {
    agent any
  environment {
    DOCKERHUB_REPOSITORY = "kyontoki/ng"
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
         docker push ${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}
        
         '''         
            }     
    }

     stage('K8S Manifest Update') {
       steps {
            git credentialsId: 'jp_git',
                url: 'https://github.com/JaeBumPark/CICD.git', /* URL변경에 따른 수정 필요 */
                branch: 'main'
            sh "git config --global user.email 'jack29@naver.com'"
            sh "git config --global user.name 'JaeBumPark'"
            sh "sed -i 's/${DOCKERHUB_REPOSITORY}.*\$${DOCKERHUB_REPOSITORY}:${BUILD_NUMBER}/g' kyo.yaml"
            sh "git add kyo.yaml"
            sh "git commit -m '[UPDATE] POD ${BUILD_NUMBER} image versioning'"
            /* sshagent (credentials: ['GitLab_SSH_Key']) {
               /*  sh "git remote set-url origin git@git.kbotest.shop:kbo/manifest.git" URL변경에 따른 수정 필요 */
            sh "git push -u origin main"
            }  
        }
    }
  } 

