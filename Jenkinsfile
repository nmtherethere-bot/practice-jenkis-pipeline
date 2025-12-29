pipeline {
    agent any

    environment {
        DOCKER_CRED = credentials('jenkins-docker-token')
    }

    tools {
        maven 'Maven-3.9'
        jdk 'JDK-17'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/nmtherethere-bot/practice-jenkis-pipeline.git'
            }
        }

        stage('Build JAR') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Verify JAR') {
            steps {
                bat 'dir target'
            }
        }

        stage('Verify Docker') {
            steps {
                bat 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t nilk3391/demo-app:latest .'
            }
        }

        stage('Login Docker Hub') {
            steps {
                bat '''
                echo %DOCKER_CRED_PSW% | docker login -u %DOCKER_CRED_USR% --password-stdin
                '''
            }
        }

        stage('Push Image') {
            steps {
                bat 'docker push nilk3391/demo-app:latest'
            }
        }
    }

    post {
        success {
            echo 'CI + Docker pipeline executed successfully 🎉'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
