// pipeline {

//     agent any

//     stages {

//         stage('Build/Push image') {

//             steps {
//                 withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
//                     sh 'docker-compose build .'
//                     sh 'sleep 5'
//                     sh 'docker push mathoanghai92/fe-crm'
//                     sh 'docker push mathoanghai92/be-crm'
//                     sh 'docker push mathoanghai92/celery-crm'
//                     sh 'docker push mathoanghai92/daphne-crm'
//                 }
//             }
//         }

//         stage('Deploy Fe') {
//             steps {
//                 echo 'Deploying and cleaning'
//                 sh 'docker image pull mathoanghai92/fe-crm'
//                 sh 'docker container stop mathoanghai92-springboot || echo "this container does not exist" '
//                 sh 'docker network create dev || echo "this network exists"'
//                 sh 'echo y | docker container prune '
//                 sh 'docker container run -d --rm --name mathoanghai92-springboot -p 8082:8082 --network dev mathoanghai92/springboot'
//             }
//         }

//         stage('Deploy Be') {
//             steps {
//                 echo 'Deploying and cleaning'
//                 sh 'docker image pull mathoanghai92/be-crm'
//                 sh 'docker container stop mathoanghai92-springboot || echo "this container does not exist" '
//                 sh 'docker network create dev || echo "this network exists"'
//                 sh 'echo y | docker container prune '
//                 sh 'docker container run -d --rm --name mathoanghai92-springboot -p 8082:8082 --network dev mathoanghai92/springboot'
//             }
//         }
 
//     }
//     post {
//         // Clean after build
//         always {
//             cleanWs()
//         }
//     }
// }

pipeline {
  agent any
 
  stages {
    stage('Checkout') {
      steps {
        echo"Checking"
      }
    }
 
    stage('Build Docker Image') {
      steps {
        script {
          // Build the Docker image using docker-compose
          sh 'docker-compose build'
        }
      }
    }
 
    stage('Push Docker Image') {
      steps {
        script {
            withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                // Push the Docker image to a Docker registry
                sh 'docker-compose push'
            }
        }
      }
    }
  }
}