pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/Hanush-14/2demo.git'
        BRANCH_NAME = 'main'   // Ensure this is the right branch
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: "*/${BRANCH_NAME}"]], 
                          userRemoteConfigs: [[url: "${GIT_REPO}"]]
                ])
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-spring-app:latest .'
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                // Add your SSH or other deployment commands here
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
