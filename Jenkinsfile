pipeline {
    agent any

    stages {
        stage('Deploy WordPress') {
            steps {
                script {
                    def namespace = 'wp'
                    def helmReleaseName = 'final-project-wp-scalefocus'
                    def chartLocation = '.' // location of the chart relative to the Jenkins workspace

                    // Check if namespace exists
                    sh(script: "kubectl get namespace ${namespace}", returnStatus: true)
                    if (result != 0) {
                        // Namespace doesn't exist, create it
                        sh "kubectl create namespace ${namespace}"
                    }

                    // Check if WordPress exists
                    sh(script: "helm list -n ${namespace} | grep ${helmReleaseName}", returnStatus: true)
                    if (result != 0) {
                        // WordPress doesn't exist, install the chart
                        sh "helm upgrade --install ${helmReleaseName} ${chartLocation} -n ${namespace} --values ./values.yaml"
                    }
                }
            }
        }
    }
}

