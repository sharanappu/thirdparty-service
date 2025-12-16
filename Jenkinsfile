pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven'
    }

    environment {
        IMAGE_NAME = "thirdparty-service"
        IMAGE_TAG  = "local"
        CONTAINER  = "thirdparty"
        HOST_PORT  = "8083"
        APP_PORT   = "8080"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                  docker rm -f $CONTAINER || true
                  docker run -d -p $HOST_PORT:$APP_PORT --name $CONTAINER $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "Third-party Service deployed successfully!"
        }
        failure {
            echo "Third-party Service build failed!"
        }
    }
}
