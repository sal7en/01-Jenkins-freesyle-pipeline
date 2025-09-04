pipeline {
    agent any

    tools {
        maven 'M399' 
    }
    
    environment {
        IMAGE_NAME = "mahmoudaboelsahen/level5-q12"
    }
    stages {

        stage('Build using Maven'){
            steps{
                //clean then create the package
                sh 'mvn clean package'
            }
        }
        
        stage('Testing'){
            steps{
                sh 'mvn test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                  echo "Building the Docker image"
                  sh """
                    docker build -t ${IMAGE_NAME}:${JOB_NAME}-${BUILD_NUMBER} .
                    """
                    
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
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
