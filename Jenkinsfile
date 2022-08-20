pipeline {
  environment {
        registry = "jerrygb7/fe-commerce"
        registryCredential = 'Dockerhub'
        dockerImage = ''
  }
  agent any
  stages {
    stage('cloning from git'){
      steps{
        git 'https://github.com/JerryGB7/e-commerce-frontend-blue.git'
      } 
    }
//     stage('stop docker containers'){
//       steps{
//         catchError(buildResult: 'SUCCESS'){
//           sh 'docker stop fe-commerce-app'
//         }
//         catchError(buildResult: 'SUCCESS'){
//           sh 'docker rm -f fe-commerce-app'
//         }
//       }
//     }
    stage('Build the image'){
      steps{
        script{
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy our image'){
      steps{
        script{
          docker.withRegistry('', registryCredential){
            dockerImage.push()
          }
        }
      }
    }
    stage('Run image'){
      steps{
        script{
          docker.withRegistry('', registryCredential){
            sh "docker run -d -p 5001:5000 --name fe-commerce-app $registry:$BUILD_NUMBER"
          }
        }
      }
    }
  }
}
