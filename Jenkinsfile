pipeline {
    agent any

    stages {
        stage('Configure Docker') {
            steps {
                script {
                    sh('eval $(minikube -p minikube docker-env)')
                }
            }
        }

        stage('Check Namespace and Deploy') {
            steps {
                script {
                    // Check if namespace 'wp' exists
                    def namespace = sh(script: "kubectl get namespaces wp --no-headers --output=go-template={{.metadata.name}}", returnStdout: true).trim()
                    // If 'wp' namespace doesn't exist, create it
                    if(namespace == "") {
                        sh "kubectl create namespace wp"
                    }

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
}
