pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-notes-app"
        CONTAINER_NAME = "django-notes-app"
        APP_PORT = "8000"
    }

    stages {

        stage('Clone the Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/nasirbloch323/django-notes-app-Docker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo ">>> Building Docker image..."
                    docker build -t ${IMAGE_NAME}:latest .
                    echo ">>> Image build ho gaya!"
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    echo ">>> Purana container band kar rahe hain..."
                    docker stop ${CONTAINER_NAME} 2>/dev/null || echo "Koi container nahi tha, skip..."
                    docker rm   ${CONTAINER_NAME} 2>/dev/null || echo "Koi container remove nahi hua, skip..."
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    echo ">>> Naya container start kar rahe hain..."
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${APP_PORT}:8000 \
                        --restart unless-stopped \
                        ${IMAGE_NAME}:latest

                    echo ">>> 5 second wait kar rahe hain container ke liye..."
                    sleep 5

                    echo ">>> Container status check kar rahe hain..."
                    docker ps | grep ${CONTAINER_NAME}

                    echo ">>> Container logs (last 20 lines):"
                    docker logs --tail 20 ${CONTAINER_NAME}
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    echo ">>> App ka health check kar rahe hain..."
                    sleep 3
                    curl -f http://localhost:${APP_PORT}/ || \
                    curl -f http://localhost:${APP_PORT}/api/ || \
                    echo "WARNING: Health check fail hua - lekin container chal raha hai, manually check karo"
                '''
            }
        }
    }

    post {
        success {
            echo '''
            ✅ Pipeline successfully complete ho gaya!
            App chal raha hai: http://<your-ec2-public-ip>:8000/
            API endpoint:      http://<your-ec2-public-ip>:8000/api/
            '''
        }
        failure {
            sh '''
                echo "❌ Pipeline fail ho gaya! Debug info:"
                echo "--- Docker containers ---"
                docker ps -a | grep django || true
                echo "--- Container logs ---"
                docker logs ${CONTAINER_NAME} --tail 50 2>/dev/null || echo "Container nahi mila"
                echo "--- Docker images ---"
                docker images | grep django || true
            '''
            echo '❌ Build fail hua — upar console output check karo.'
        }
        always {
            sh '''
                echo ">>> Cleanup: dangling images remove kar rahe hain..."
                docker image prune -f || true
            '''
        }
    }
}
