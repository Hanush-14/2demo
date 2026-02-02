pipeline {
    agent any

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64'
        MAVEN_HOME = '/usr/local/maven'
        PATH = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }

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
                sh 'docker build -t my-app:${BRANCH_NAME} .'
            }
        }

        stage('Deploy to Remote Server') {
            steps {
                sh 'scp target/*.jar user@remote-server:/path/to/deploy'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
