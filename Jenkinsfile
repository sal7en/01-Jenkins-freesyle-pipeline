pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
        IMAGE_NAME = "yourdockerhubusername/myapp"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                  echo "Building the Docker image..."
                  sh 'docker build -t ${IMAGE_NAME}:${JOB_NAME}-${BUILD_NUMBER} . '
                    
                }
            }
        }
        stage('Build and Push Docker Image') {
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
