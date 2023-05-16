pipeline {
    agent any

    stages {
        stage('Install Kubectl and Helm') {
            steps {
                sh('curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl')
                sh('chmod +x ./kubectl')
                sh('sudo mv ./kubectl /usr/local/bin/kubectl')
                sh('curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3')
                sh('chmod 700 get_helm.sh')
                sh('./get_helm.sh')
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh('helm version')
                sh('kubectl version')
                // Your deployment steps here
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
