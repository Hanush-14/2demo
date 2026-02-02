pipeline {
    agent any

    environment {
        REMOTE_HOST = 'your-remote-host'      // Replace with your remote server
        REMOTE_USER = 'your-ssh-username'     // Replace with your SSH username
        DOCKER_IMAGE = 'my-spring-app:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Hanush-14/2demo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                sshCommand remote: [
                    name: 'REMOTE',           // Jenkins SSH server name configured in credentials
                    host: "${REMOTE_HOST}",
                    user: "${REMOTE_USER}",
                    allowAnyHosts: true       // Avoid host verification issues
                ],
                command: 'mvn clean package'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build and Deploy succeeded!'
        }
        failure {
            echo 'Build or Deploy failed!'
        }
    }
}
