pipeline {
    agent any

    environment {
        ECR_REGISTRY = ''
        APP_REPO = ''
        MONGODB_REPO = ''
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Debayanmondal/final-project.git'
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    docker.withRegistry("${ECR_REGISTRY}", 'ecr:us-east-1:aws-credentials') {
                        def appImage = docker.build("${APP_REPO}:latest", "-f Dockerfile .")
                        def mongoImage = docker.build("${MONGODB_REPO}:latest", "-f mongodb.Dockerfile .")

                        appImage.push()
                        mongoImage.push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetes.kubernetesDeploy(
                        kubeconfigId: 'kubernetes-config',
                        manifests: [
                            'app-deployment.yaml',
                            'app-service.yaml',
                            'mongodb-deployment.yaml',
                            'mongodb-service.yaml'
                        ]
                    )
                }
            }
        }
    }
}
