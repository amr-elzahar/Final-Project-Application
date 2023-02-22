pipeline {
   agent {label 'jenkins-slave'}

   environment {
      DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
   }

   stages {
      stage('Build Image'){
         steps {
            sh 'docker build -t amrelzahar/simple-app .'
         }
      }

      stage('Login') {
         steps {
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
         }
      }

      stage('Push Image'){
         steps {
            sh 'docker push amrelzahar/simple-app'
         }
      }

      stage('deploy') {
         steps {
         
               withCredentials([file(credentialsId: 'myconfig', variable: 'config')]) {
               sh """
                     kubectl apply -f Deployment --kubeconfig=${config}
                  """
            }
         }
      }
   }
}