pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mohamedathikr/my-node-app:latest"
        K8S_DEPLOYMENT = "node-app-deployment.yaml"
        K8S_SERVICE = "node-app-service.yaml"
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                echo "🧹 Cleaning workspace..."
                cleanWs() // Cleans workspace before fetching new code
            }
        }

        stage('Clone Repository') {
            steps {
                echo "📥 Cloning repository..."
                git branch: 'main', url: 'https://github.com/ATHIK05/DevOps_Task06.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "🐳 Building Docker Image: $env.DOCKER_IMAGE"
                    powershell """
                        echo '🚀 Starting Docker Build...'
                        docker build -t mohamedathikr/my-node-app:latest .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo "📤 Pushing Docker Image to Docker Hub..."
                    withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                        powershell """
                            echo '🔄 Logging in to Docker Hub...'
                            docker login -u mohamedathikr -p qwerty786!A
                            echo '🚀 Pushing Image: $env:DOCKER_IMAGE'
                            docker push mohamedathikr/my-node-app:latest
                        """
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    echo "🚀 Deploying to Kubernetes..."
                    powershell """
                        echo '📂 Applying Deployment: $env:K8S_DEPLOYMENT'
                        kubectl apply -f $env:K8S_DEPLOYMENT
                        echo '📂 Applying Service: $env:K8S_SERVICE'
                        kubectl apply -f $env:K8S_SERVICE
                    """
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo "🔍 Verifying Kubernetes Deployment..."
                    powershell """
                        echo '📌 Listing Pods...'
                        kubectl get pods
                        echo '📌 Listing Services...'
                        kubectl get svc
                    """
                }
            }
        }
    }
}
