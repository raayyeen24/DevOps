
pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Recuperation du code') {
            steps {
                checkout scm
            }
        }

        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t backend-app:latest -f backend/Dockerfile .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_TOKEN'
                )]) {
                    sh '''
                        echo "$DOCKERHUB_TOKEN" | docker login -u "$DOCKERHUB_USER" --password-stdin
                        docker tag backend-app:latest "$DOCKERHUB_USER/backend-app:latest"
                        docker push "$DOCKERHUB_USER/backend-app:latest"
                        docker logout
                    '''
                }
            }
        }
    }
}
