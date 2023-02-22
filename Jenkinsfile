pipeline {
   agent {label 'jenkins-slave'}

   environment {
      DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
   }

   stages {
      stage('Necessary Commands'){
         steps {

            // Start ssh service
            sh 'service ssh start'

            // Modify docker.sock
            sh 'chmod 666 /var/run/docker.sock'

            // Install gcloud
            sh """
                  echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
                  
                  curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
               
                  apt-get update && apt-get install google-cloud-cli
               """
         }
      }

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
         
               withCredentials([file(credentialsId: 'cluster-config', variable: 'config')]) {
               sh """
                     kubectl apply -f deployment --kubeconfig=${config}
                  """
            }
         }
      }
   }
}