
pipeline {
    agent any

    tools {
        jdk 'jdk11'
        maven 'maven3'
    }

    environment {
        DOCKER_IMAGE = 'JESS/spring-petclinic:latest'  // Nom de l'image Docker
        DOCKER_REGISTRY = 'https://hub.docker.com'  // L'URL de DockerHub
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false,url: 'https://github.com/NarjesTaghlet/Petclinic.git'
            }
        }


        stage('Build') {
            steps {
                script {
                    // Construire avec Maven
                    sh 'mvn clean install'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    // Exécuter des tests, par exemple
                    sh 'mvn test'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Construire l'image Docker
                    sh 'docker build -t ${DOCKER_IMAGE} .'  // Construire l'image à partir du Dockerfile
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Authentifier avec DockerHub et pousser l'image construite
                    docker.withRegistry('https://hub.docker.com', 'dockerhub-credentials') {
                        // Pousser l'image Docker sur DockerHub
                        sh 'docker push ${DOCKER_IMAGE}'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                // Étape de déploiement ici
                echo 'Déploiement en cours...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé'
        }
        success {
            echo 'Le pipeline a réussi avec succès.'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}


