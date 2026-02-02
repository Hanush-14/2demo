pipeline {
    agent any

    environment {
        IMAGE_NAME = "hanush14/sam123"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Hanush-14/2demo.git'
            }
        }

        stage('Maven Build'){
          steps{
                ssh 'mvn clean package'
                }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.with.Registry('https"//index.docker.io/v1/','dockerhub'){
                    dockerImage.push()\
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded!"
        }
        failure {
            echo "Pipeline failed!"
        }
        always {
            deleteDir()
        }
    }
}
