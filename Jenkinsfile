pipeline {
    agent any

    environment {
        REGISTRY = "docker.io"   
        IMAGE_NAME = "maven-app"       
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Unit Tests') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') { 
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $REGISTRY/$IMAGE_NAME:${env.BUILD_NUMBER} ."
                }
            }
        }

        stage('Trivy Security Scan') {
            steps {
                script {
                    sh "trivy image $REGISTRY/$IMAGE_NAME:${env.BUILD_NUMBER} > trivy-report.txt"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "docker login -u $DOCKER_USER -p $DOCKER_PASS $REGISTRY"
                        sh "docker push $REGISTRY/$IMAGE_NAME:${env.BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig([credentialsId: 'k8s-credentials', contextName: 'k8s-cluster']) {
                        sh "kubectl set image deployment/$IMAGE_NAME $IMAGE_NAME=$REGISTRY/$IMAGE_NAME:${env.BUILD_NUMBER}"
                        sh "kubectl rollout status deployment/$IMAGE_NAME"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed!'
        }
    }
}
