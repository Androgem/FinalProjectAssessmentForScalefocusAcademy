pipeline {
   agent any

   environment {
      KUBECONFIG = '//wsl.localhost/Ubuntu-20.04/home/andrej/.kube/config'
   }

   stages {
      stage('Namespace') {
            steps {
               script {
                  def namespaceExists = sh(script: 'kubectl get namespace wp', returnStatus: true) == 0
                  if (namespaceExists) {
                        echo 'Namespace wp exists'
                        return
                  } else {
                        echo 'Creating wp namespace'
                        sh 'kubectl create namespace wp'
                  }
               }
            }
      }

      stage('Helm') {
            steps {
               script {
                  sh 'helm dependency build ./bitnami/wordpress'
                  sh 'helm upgrade --install final-project-wp-scalefocus ./bitnami/wordpress -n wp'
               }
            }
      }
   }
}
