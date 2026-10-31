pipeline{
  agent any
  environment{
    IMAGE_NAME = 'karthikeyan0602/jenkins-docker-demo'
  }
  stages {
    stage('Checkout'){
      steps{
        echo 'Getting Source code'
        checkout scm
      }
    }
    stage('Docker version'){
      steps{
        sh 'docker --version'
      }
    }
    stage('Build the docker image')
    {
      steps{
        sh 'docker build -t "$IMAGE_NAME:1" .'
      }
    }
    stage('Docker Login'){
      steps{
        withCredentials([
          usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER' ,
            passwordVariable: 'DOCKER_PASS' ,
            )
          ]){
          sh ''' 
          set +x
          echo "$DOCKER_PASS" | docker login \
          --username "DOCKER_USER" \
          --password-stdin
          '''
        }
      }
    }
    stage('push the image to the Docker'){
      steps{
        sh ''' 
           docker push "$IMAGE_NAME:1"
           '''
      }
    }
  }
post {
  success{
    echo 'Docker image successfully publised'
  }
  failure{
    echo 'Pipeline Failed'
  }
  always{
    sh 'docker logout || true'
  }
}
}

  









        









    
  

