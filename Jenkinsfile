pipeline {
    agent any

    stages {
        stage('Check Helm and Kubectl') {
            steps {
                script {
                    // Install Helm
                    sh '''
                        curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
                        sudo apt-get install apt-transport-https --yes
                        echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
                        sudo apt-get update
                        sudo apt-get install helm --yes
                    '''

                    // Install Kubectl
                    sh '''
                        sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
                        curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
                        echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
                        sudo apt-get update
                        sudo apt-get install -y kubectl
                    '''
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
