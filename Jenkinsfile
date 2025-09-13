pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5' // Define this in Jenkins global tools
        jdk 'JDK 17'        // Also define this in Jenkins tools
    }

    environment {
        IMAGE_NAME = "bookmyshow-app"
        DOCKER_REGISTRY = "your-dockerhub-username"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/reddyhari3333-art/book-my-show.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_REGISTRY/$IMAGE_NAME .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $DOCKER_REGISTRY/$IMAGE_NAME'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy logic goes here (e.g., kubectl apply)'
            }
        }
    }
}
