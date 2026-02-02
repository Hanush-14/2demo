pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-spring-app:latest'  // Replace with your image name
        REMOTE_SERVER = 'my-remote-server'    // Replace with your Jenkins remote server name
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from GitHub
                git 'https://github.com/Hanush-14/2demo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                // Build the project using Maven
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image (optional stage)
                    // Make sure your Dockerfile is properly set up for the image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                // SSH into the remote server and run the build command
                ssh(
                    remote: REMOTE_SERVER,  // Replace with your Jenkins configured remote server
                    command: 'mvn clean package'  // Run on the remote server
                )
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // If you want to push the Docker image to DockerHub or your registry
                    // Uncomment the following line and replace with your registry details
                    // sh 'docker push my-spring-app:latest'
                }
            }
        }

    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
        success {
            // Notify when the build is successful
            echo 'Build succeeded!'
        }
        failure {
            // Notify when the build fails
            echo 'Build failed!'
        }
    }
}
