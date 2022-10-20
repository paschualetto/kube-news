pipeline {
    agent any

    stages {
        stage("Docker image Build") {
            steps {
                script {
                    dockerapp = docker.build("paschualetto/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
                }
            }
        }
    }
}