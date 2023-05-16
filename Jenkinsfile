pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git 'https://github.com/bitnami/charts.git'
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    def namespace = 'wp'
                    def helmReleaseName = 'final-project-wp-scalefocus'
                    def chartLocation = 'bitnami/wordpress' // location of the chart relative to the Jenkins workspace

                    // Check if namespace exists
                    def result = sh(script: "kubectl get namespace ${namespace}", returnStatus: true)
                    if (result != 0) {
                        // Namespace doesn't exist, create it
                        sh "kubectl create namespace ${namespace}"
                    }

                    // Check if WordPress exists
                    result = sh(script: "helm list -n ${namespace} | grep ${helmReleaseName}", returnStatus: true)
                    if (result != 0) {
                        // WordPress doesn't exist, install the chart
                        sh "helm upgrade --install ${helmReleaseName} ${chartLocation} -n ${namespace} --values ${chartLocation}/values.yaml"
                    }
                }
            }
        }
    }
}
