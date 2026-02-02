pipeline {
    agent any

    environment {
        MAVEN_HOME = '/usr/local/maven' // Set the path to Maven if needed
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk-amd64' // Set the path to Java if needed
    }

    stages {
        // Stage 1: Checkout the source code from GitHub repository
        stage('Checkout SCM') {
            steps {
                // Checkout the source code from the Git repository
                checkout scm
            }
        }

        // Stage 2: Build with Maven
        stage('Build with Maven') {
            steps {
                script {
                    // Build different branches with different Maven goals
                    if (env.BRANCH_NAME == 'main') {
                        echo "Building main branch with mvn clean package"
                        sh 'mvn clean package' // Use Maven clean package for the main branch
                    } else if (env.BRANCH_NAME == 'dev') {
                        echo "Building dev branch with mvn clean install"
                        sh 'mvn clean install' // Use Maven clean install for dev branch
                    } else {
                        echo "Building other branch with mvn clean verify"
                        sh 'mvn clean verify' // Default build command for other branches
                    }
                }
            }
        }

        // Stage 3: Build Docker Image (Optional - Can be skipped based on requirements)
        stage('Build Docker Image') {
            steps {
                script {
                    // Example: Build the Docker image using a Dockerfile
                    echo "Building Docker image for branch ${env.BRANCH_NAME}"
                    sh 'docker build -t my-app:${env.BRANCH_NAME} .' // Customize based on your requirements
                }
            }
        }

        // Stage 4: Deploy to Remote Server (Optional - Can be skipped based on requirements)
        stage('Deploy to Remote Server') {
            steps {
                script {
                    // Example: Deploy the application to a remote server
                    echo "Deploying to remote server for branch ${env.BRANCH_NAME}"
                    sh 'scp target/my-app.jar user@remote-server:/path/to/deploy' // Customize based on your environment
                }
            }
        }

    }

    post {
        // Cleanup workspace after the build, always runs even if the build fails
        always {
            echo 'Cleaning up workspace...'
            cleanWs() // Cleans the workspace to remove any leftover files
        }

        // Notify on success
        success {
            echo 'Build completed successfully!'
        }

        // Notify on failure
        failure {
            echo 'Build failed!'
        }
    }
}
