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
    }
}
