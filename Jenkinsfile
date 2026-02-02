pipeline {
    agent any

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    echo "Checking Java and Maven versions"
                    sh 'java -version'
                    sh 'mvn -version'

                    if (env.BRANCH_NAME == 'main') {
                        echo "Building main branch"
                        sh 'mvn clean package'
                    } else if (env.BRANCH_NAME == 'dev') {
                        echo "Building dev branch"
                        sh 'mvn clean install'
                    } else {
                        echo "Building other branch"
                        sh 'mvn clean verify'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use 'latest' if BRANCH_NAME is empty
                    def tag = env.BRANCH_NAME ?: 'latest'
                    echo "Building Docker image with tag: ${tag}"
                    sh "docker build -t my-app:${tag} ."
                }
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                echo "Deploying application to remote server"
                sh 'scp target/*.jar user@remote-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
