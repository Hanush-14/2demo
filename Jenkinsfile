pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "dockerhub_username/my-spring-app"
        DOCKER_TAG   = "latest"
        DOCKER_CREDS = "dockerhub-creds"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh '''
                  echo "Java Version"
                  java -version

                  echo "Maven Version"
                  mvn -version

                  mvn clean package
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDS,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                  docker push $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                  docker service rm my-spring-service || true

                  docker service create \
                    --name my-spring-service \
                    --replicas 2 \
                    -p 8080:8080 \
                    $DOCKER_IMAGE:$DOCKER_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully"
        }
        failure {
            echo "❌ Pipeline failed"
        }
        always {
            cleanWs()
        }
    }
}
