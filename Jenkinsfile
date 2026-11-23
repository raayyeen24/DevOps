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
    }
}
