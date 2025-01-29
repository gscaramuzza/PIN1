pipeline {
  agent any

  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "elbuo8/webapp:${env.BUILD_NUMBER}"
  }
   stages {
   stage('Building image') {
      steps{
          sh '''
          docker build -t testapp .
             '''  
        }
    }
  
  
    stage('Run tests') {
      steps {
        sh "docker run testapp npm test"
      }
    }
    stage('Deploy Image') {
        steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
            sh '''
              echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
              docker tag testapp gscaramuzza/testapp:latest
              docker push gscaramuzza/testapp:latest
            '''
          }
        }
      }
    }
}


    
  

