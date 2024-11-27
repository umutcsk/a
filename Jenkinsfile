pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "umutcskn681/helloworldapp"
        VERSION = "1.${BUILD_NUMBER}" 
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${VERSION} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker login -u your-dockerhub-username -p Um.738770"
                sh "docker push ${DOCKER_IMAGE}:${VERSION}"
            }
        }
        stage('Run Docker Container') {
            steps {
                sh "docker run -d -p 3000:3000 --name nodejs-app ${DOCKER_IMAGE}:${VERSION}"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f deployment.yaml"
                sh "kubectl apply -f service.yaml"
                sh "kubectl set image deployment/nodejs-hello-world nodejs-container=${DOCKER_IMAGE}:${VERSION}"
            }
        }
    }
}
