pipeline {
    agent any

    stages {
        stage ('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("wpalmeida/nginx", '-f .Dockerfile')
                }
            }
        }
    }
}
