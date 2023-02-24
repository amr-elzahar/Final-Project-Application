pipeline {
   agent {label 'jenkins-slave'}

   environment {
      DOCKERHUB_CREDENTIALS = credentials('dockerhub_creds')
   }

   stages {
      
      stage('Build Image'){
         steps {
            sh 'docker build -t amrelzahar/node-application .'
         }
      }

      stage('Login') {
         steps {
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
         }
      }

      stage('Push Image'){
         steps {
            sh 'docker push amrelzahar/node-application'
         }
      }

      stage('deploy') {
         steps {
         
               withCredentials([file(credentialsId: 'cluster_config', variable: 'config')]) {
               sh """
                     kubectl apply -f deployment --kubeconfig=${config}
                  """
            }
         }
      }
   }
}