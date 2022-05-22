pipeline {
    agent any

    stages {
        stage ('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("wpalmeida/nginx:${env.BUILD_ID}")
                }
            }
        }
        stage ('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage ('Setup OCI CLI') {
            steps {
                sh 'wget https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh'
                sh 'bash install.sh'
            }
        }
        stage ('Deploy Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }
}
