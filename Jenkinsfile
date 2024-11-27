pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "umutcskn681/node"
        VERSION = "1.${BUILD_NUMBER}"
        KUBECONFIG = '/home/test/.kube/config'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${VERSION} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker login -u umutcskn681 -p Um.738770"
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
                script {
                    sh "kubectl --kubeconfig=${KUBECONFIG} apply -f deployment.yaml"
                    sh "kubectl --kubeconfig=${KUBECONFIG} apply -f service.yaml"
                    sh "kubectl --kubeconfig=${KUBECONFIG} set image deployment/nodejs-hello-world nodejs-container=${DOCKER_IMAGE}:${VERSION}"
                }
            }
        }
    }
}
