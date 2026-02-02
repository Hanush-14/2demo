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
                    sh 'java -version'
                    sh 'mvn -version'

                    if (env.BRANCH_NAME == 'main') {
                        sh 'mvn clean package'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'mvn clean install'
                    } else {
                        sh 'mvn clean verify'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def tag = env.BRANCH_NAME ?: 'latest'
                    sh "docker build -t my-app:${tag} ."
                }
            }
        }

        stage('Deploy to Remote Server') {
            when {
                expression { false }   // 🔒 disabled safely
            }
            steps {
                echo 'Deploy stage disabled'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo '✅ PIPELINE SUCCESS'
        }
        failure {
            echo '❌ PIPELINE FAILED'
        }
    }
}
