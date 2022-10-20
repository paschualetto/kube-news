pipeline {
    agent any

    stages {
        stage("Docker Image Build") {
            steps {
                script {
                    dockerapp = docker.build("paschualetto/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
                }
            }
        }
        stage("Docker Image Push") {
            steps {
                script {
                    docker.withRegistry("https://index.docker.io/v1/", "dockerhub") {
                        dockerapp.push("latest")
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}