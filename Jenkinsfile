pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "votreUser"             // <- remplace par ton nom Docker Hub
        DOCKERHUB_CREDENTIALS = "dockerhub-cred" // <- ID du credential Jenkins
        IMAGE_NAME = "student-management"
        IMAGE_TAG = "v1.0"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Récupération du code source..."
                checkout scm
            }
        }

        stage('Build project (Maven)') {
            steps {
                echo "Compilation du projet..."
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Construction de l'image Docker..."
                sh """
                    docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
                    docker tag ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                """
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo "Connexion à Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: "USER", passwordVariable: "PASS")]) {
                    sh "echo \$PASS | docker login -u \$USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Push de l'image vers Docker Hub..."
                sh """
                    docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        success {
            echo "✔ Build + Docker Push réalisés avec succès !"
        }
        failure {
            echo "❌ Une erreur est survenue."
        }
    }
}
