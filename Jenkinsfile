pipeline {
    agent any

    environment {
        KUBECONFIG = '/home/andrej/.kube/config'
    }
    stages {
        stage('Check Namespace') {
            steps {
                script {
                    // Check if namespace 'wp' exists
                    def namespace = sh(script: "kubectl get namespace wp", returnStatus: true) ? 'wp' : ''
                    // If 'wp' namespace doesn't exist, create it
                    if (namespace.isEmpty()) {
                        sh "kubectl create namespace wp"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Check if WordPress is already installed
                    def release = sh(script: "helm dependency build ./wordpress", returnStatus: true) ? 'true' : ''
                    // If WordPress is not installed, install it
                    if (release.isEmpty()) {
                        sh "helm upgrade --install final-project-wp-scalefocus ./wordpress -n wp"
                    }
                }
            }
        }
    }
}
