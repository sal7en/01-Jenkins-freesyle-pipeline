pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
        IMAGE_NAME = "mahmoudaboelsalhen/level3-q6"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                  echo "Building the Docker image..."
                  sh 'docker build -t ${IMAGE_NAME}:1.0 . '
                    
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    
                    withCredentials([usernamePassword(credentialsId: env.DOCKER_HUB_CREDENTIALS, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh """
                        echo "${PASSWORD}" | docker login -u "${USERNAME}" --password-stdin
                        docker push ${IMAGE_NAME}:${JOB_NAME}-${BUILD_NUMBER}
                        """
                    }
                }
            }
        }
    }
}
