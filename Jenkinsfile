pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-app:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("my-app:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    try {
                        sh 'docker stop my-app-container || true && docker rm my-app-container || true'
                    } catch (Exception e) {
                        echo "No existing container to stop/remove."
                    }

                    sh '''
                    docker run -d --name my-app-container -p 3000:3000 my-app:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}
