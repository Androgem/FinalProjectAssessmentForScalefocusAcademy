pipeline {
    agent any
    stages {
        stage('Deploy WordPress') {
            steps {
                script {
                    def namespace = 'wp'
                    def helmRelease = 'final-project-wp-scalefocus'
                    def exists = sh(returnStatus: true, script: "kubectl get ns ${namespace}") == 0
                    if (!exists) {
                        sh "kubectl create ns ${namespace}"
                    }
                    exists = sh(returnStatus: true, script: "helm list -n ${namespace} | grep ${helmRelease}") == 0
                    if (!exists) {
                        sh "helm install ${helmRelease} ./wordpress -n ${namespace}"
                    }
                }
            }
        }
    }
}
