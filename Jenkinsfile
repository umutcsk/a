pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "umutcskn681/node-app"
        VERSION = "1.${BUILD_NUMBER}"
        KUBECONFIG = '~/.kube/config'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${DOCKER_IMAGE}:${VERSION} .
                    docker run -d -p 3000:3000 --name nodejs-container ${DOCKER_IMAGE}:${VERSION}
                """
            }
        }
        stage('Push Docker Image') {
            steps {
                sh """
                    docker login -u umutcskn681 -p Um.738770
                    docker push ${DOCKER_IMAGE}:${VERSION}
                """
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh """
                        export KUBECONFIG=${KUBECONFIG} && kubectl apply -f deployment.yaml
                        export KUBECONFIG=${KUBECONFIG} && kubectl apply -f service.yaml
                    """
                }
            }
        }
    }
}
