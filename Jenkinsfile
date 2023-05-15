pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }
    stages {
        stage('Check Namespace') {
            steps {
                script {
                    // Check if namespace 'wp' exists
                    def namespace = sh(script: "kubectl get namespaces wp", returnStatus: true) ? 'wp' : ''
                    // If 'wp' namespace doesn't exist, create it
                    if (namespace.isEmpty()) {
                        sh "kubectl create namespace wp"
                    }
                }
            }
        }
        stage('Deploy')
        steps {
            script {
                // Check if WordPress is already installed
                def release = sh(script: "helm list -n wp -f final-project-wp-scalefocus --output json | jq -r '.[0].name'", returnStdout: true).trim()
                // If WordPress is not installed, install it
                if(release == "null") {
                    sh "helm install final-project-wp-scalefocus ./charts/bitnami/wordpress -n wp"
                }
            }
        }
    }
}
