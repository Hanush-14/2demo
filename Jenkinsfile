pipeline {
    agent any

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test using Maven') {
            steps {
                echo 'Building Java application using Maven'
                sh 'java -version'
                sh 'mvn -version'
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t my-spring-app:latest .'
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                echo 'Deploying application to Docker Swarm'
                sh '''
                    docker service rm my-spring-service || true

                    docker service create \
                      --name my-spring-service \
                      --replicas 2 \
                      -p 8080:8080 \
                      my-spring-app:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline executed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            cleanWs()
        }
    }
}
