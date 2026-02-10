pipeline {
    agent any

    environment {
        IMAGE_NAME = "amazon"
        CONTAINER_NAME = "azure"
        HOST_PORT = "6060"
        CONTAINER_PORT = "8080"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/Debasishtewary08/pipeline.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Remove Old Image') {
            steps {
                sh '''
                docker rmi $IMAGE_NAME || true
                '''
            }
        }

        stage('Docker Image Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Docker Deploy') {
            steps {
                sh '''
                docker run -d \
                -p $HOST_PORT:$CONTAINER_PORT \
                --name $CONTAINER_NAME \
                $IMAGE_NAME
                '''
            }
        }
    }
}
