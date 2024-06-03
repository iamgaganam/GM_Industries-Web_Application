pipeline {
    agent any
    tools {
        git 'Default' // Replace 'Default Git' with the name you configured in Jenkins for the Git tool
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerdocker') // Replace 'your-dockerhub-credentials-id' with the ID of your Docker Hub credentials
    }
    stages {
        stage('Declarative: Tool Install') {
            steps {
                echo 'Tool installation stage'
            }
        }
        stage('SCM Checkout') {
            steps {
                script {
                    git url: 'https://github.com/iamgaganam/GM_Industries_Web_Application.git', branch: 'main'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('gm_industries_web_app')
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        echo 'Logged in to Docker Hub'
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        docker.image('gm_industries_web_app').push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                try {
                    bat 'docker logout'
                } catch (Exception e) {
                    echo 'Docker logout failed, but continuing...'
                }
            }
        }
    }
}
