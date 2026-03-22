pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = "majithas/todo-frontend"
        BACKEND_IMAGE  = "majithas/todo-backend"
        HELM_DIR       = "helm"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'feature/devops',
                    url: 'https://github.com/majithabhanus/KOPS-HELM-CHART-PROJECT.git',
                    credentialsId: 'github-cred'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                sh "docker build -t ${FRONTEND_IMAGE}:${BUILD_NUMBER} ./ui"
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                sh "docker build -t ${BACKEND_IMAGE}:${BUILD_NUMBER} ./server"
            }
        }

        stage('Login DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred-id',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin https://index.docker.io/v1/
                    '''
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                sh """
                docker push ${FRONTEND_IMAGE}:${BUILD_NUMBER}
                docker push ${BACKEND_IMAGE}:${BUILD_NUMBER}
                """
            }
        }

        stage('Deploy using Helm') {
            steps {
                sh """
                helm upgrade --install todo-app ${HELM_DIR} \
                --namespace default \
                --create-namespace \
                --set frontend.image=${FRONTEND_IMAGE}:${BUILD_NUMBER} \
                --set backend.image=${BACKEND_IMAGE}:${BUILD_NUMBER}
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                kubectl get pods -o wide
                kubectl get svc
                kubectl get ingress
                '''
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
    }

    post {
        success {
            echo "✅ Application successfully deployed to Kubernetes"
        }
        failure {
            echo "❌ Pipeline failed. Check logs"
        }
        always {
            echo "Pipeline execution finished"
        }
    }
}
