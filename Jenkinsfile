node {
    stage('Initial Debug') {
        echo "Scripted Pipeline started. Build: ${env.BUILD_NUMBER}"
        echo "Working Directory: ${pwd()}"
    }

    stage('Checkout') {
        checkout scm
    }

    stage('Build Docker Image') {
        script {
            sh "docker build -t my-nginx-web:${env.BUILD_NUMBER} -t my-nginx-web:latest ."
        }
    }

    stage('Deploy to Kubernetes') {
        script {
            sh "kubectl apply -f k8s/deployment.yaml"
            sh "kubectl apply -f k8s/service.yaml"
            sh "kubectl apply -f k8s/ingress.yaml"
            sh "kubectl set image deployment/nginx-deployment nginx-container=my-nginx-web:${env.BUILD_NUMBER}"
        }
    }

    stage('Verify Deployment') {
        script {
            sh "kubectl rollout status deployment/nginx-deployment"
            sh "kubectl get pods -l app=my-nginx"
            sh "kubectl get svc nginx-service"
            sh "kubectl get ingress nginx-ingress"
        }
    }
}
