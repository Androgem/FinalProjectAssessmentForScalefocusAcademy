pipeline {
    agent any

    stages {
        stage('Check Helm and Kubectl') {
            steps {
                script {
                    sh '/usr/local/bin/helm version'
                    sh '/usr/local/bin/kubectl version'
                }
            }
        }
        
        stage('Git Clone') {
            steps {
                git 'https://github.com/Androgem/FinalProjectAssessmentForScalefocusAcademy.git'
            }
        }

        stage('Deploy WordPress') {
            steps {
                script {
                    def namespace = 'wp'
                    def helmReleaseName = 'final-project-wp-scalefocus'
                    def chartLocation = 'wordpress' // location of the chart relative to the Jenkins workspace

                    // Check if namespace exists
                    def result = sh(script: "/usr/local/bin/kubectl get namespace ${namespace}", returnStatus: true)
                    if (result != 0) {
                        // Namespace doesn't exist, create it
                        sh "/usr/local/bin/kubectl create namespace ${namespace}"
                    }

                    // Check if WordPress exists
                    result = sh(script: "/usr/local/bin/helm list -n ${namespace} | grep ${helmReleaseName}", returnStatus: true)
                    if (result != 0) {
                        // WordPress doesn't exist, install the chart
                        sh "/usr/local/bin/helm upgrade --install ${helmReleaseName} ${chartLocation} -n ${namespace} --values ${chartLocation}/values.yaml"
                    }
                }
            }
        }
    }
}
