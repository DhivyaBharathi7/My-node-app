pipeline {
    agent any  

    environment {
        DOCKER_IMAGE = "my-node-app"
        REGISTRY = "docker.io"
        K8S_CLUSTER = "your-k8s-cluster"
        DEPLOYMENT_NAME = "my-node-app-deployment"
    }

    stages {
        stage('Checkout') {
            steps {
        
                git 'https://github.com/DhivyaBharathi7/My-node-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                script {
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests (if any)
                script {
                    sh 'npm test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push Docker image to a registry (e.g., Docker Hub)
                script {
                    sh "docker tag $DOCKER_IMAGE $REGISTRY/$DOCKER_IMAGE"
                    sh "docker push $REGISTRY/$DOCKER_IMAGE"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Deploy the Docker image to Kubernetes
                script {
                    sh "kubectl set image deployment/$DEPLOYMENT_NAME my-node-app=$REGISTRY/$DOCKER_IMAGE --record"
                }
            }
        }
    }

    post {
        success {
            // Notify success (e.g., send a Slack message, email, etc.)
            echo 'Deployment was successful!'
        }
        failure {
            // Notify failure
            echo 'Deployment failed.'
        }
    }
}
