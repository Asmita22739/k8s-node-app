pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = 'docker-cred'
        DOCKER_IMAGE = 'asm789/k8s-node-app:latest'
    }
    stages {
        stage('Checkout') { steps { git branch: 'main', url: 'https://github.com/Asmita22739/k8s-node-app.git' } }
        stage('Build Docker') { steps { script { docker.build(DOCKER_IMAGE) } } }
        stage('Push Docker') {
            steps { script { docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) { docker.image(DOCKER_IMAGE).push() } } }
        }
        stage('Deploy to Kubernetes') {
            steps { sh 'kubectl apply -f k8s/deployment.yaml && kubectl apply -f k8s/service.yaml' }
        }
        stage('Test') { steps { sh 'kubectl get pods && kubectl get svc' } }
    }
}
