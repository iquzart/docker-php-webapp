pipeline {
  environment {
    registry = "diquzart/php-webapp"
    registryCredential = 'cbd03118-5be4-4f5a-82d8-397dbe9c508b'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/iquzart/docker-php-webapp.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image to Docker Hub') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove running version of the application') {
       steps{
        sh "docker rm -f php-web-app"
       }
     }

    stage('Depoly container with newly created image') {
       steps{
        sh "docker run -d -p 9000:80 --name 'php-web-app' diquzart/php-webapp:$BUILD_NUMBER"
       }
     }
/*   stage('Remove Unused docker image') {
     steps{
       sh "docker rmi $registry:$(echo $BUILD_NUMBER-1 | bc)"
     }
    } */
  }
}
