pipeline {
    agent any

    stages {

        stage ('Build Dcoker Image') {
            steps {
                script {
                    dcokerapp = docker.Build("fabricioveronez/kube-news:v1", '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
        stage ('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.dcoker.com', 'dockerhub' ){
                        dockerapp.push('latest')
                        dockerapp.push("${env.Build_ID}")
                        }
                } 
            }
        }
    
        stage ('Deploy Kubernetes') {
            environment {
                tag_version = ""
            }
            steps {
                withKubeconfig ([credentialsId: 'Kubeconfig']) {
                    sh 'Kubectl apply -f ./k8s/deployment.yaml'
             }
        }
    }
}
