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
                    docker.withRegistry("https://registry.hub.docker.com", "dockerhub") {
                        dockerapp.push("latest")
                        dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage("Deploy K8s") {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                script {
                    withKubeConfig([credentialsId: 'kubeconfig']) {
                        sh "kubectl apply -f ./k8s/postgre.yaml"
                        sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/app.yaml'
                        sh "kubectl apply -f ./k8s/app.yaml"
                    }
                }
            }
        }
    }
}