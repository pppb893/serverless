node {
    // Define environment variables for scripted pipeline
    def APP_NAME = 'my-nginx-web'
    def IMAGE_TAG = "${env.BUILD_NUMBER}"

    stage('Initial Debug') {
        echo "Scripted Pipeline started. Build: ${IMAGE_TAG}"
        echo "Working Directory: ${pwd()}"
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Build Docker Image') {
        sh "docker build -t ${APP_NAME}:${IMAGE_TAG} -t ${APP_NAME}:latest ."
    }

    stage('Deploy to Kubernetes') {
        sh "kubectl apply -f k8s/deployment.yaml"
        sh "kubectl apply -f k8s/service.yaml"
        sh "kubectl apply -f k8s/ingress.yaml"
        sh "kubectl set image deployment/nginx-deployment nginx-container=${APP_NAME}:${IMAGE_TAG}"
    }

    stage('Verify Deployment') {
        sh "kubectl rollout status deployment/nginx-deployment"
        sh "kubectl get pods -l app=my-nginx"
        sh "kubectl get svc nginx-service"
        sh "kubectl get ingress nginx-ingress"
    }
}
