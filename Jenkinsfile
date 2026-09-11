pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t travelgo-backend ./backend'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                    docker stop travelgo-backend || true
                    docker rm travelgo-backend || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker run -d \
                    --name travelgo-backend \
                    --add-host=host.docker.internal:host-gateway \
                    --env-file /home/ubuntu/TripGo/backend/.env \
                    -p 4000:4000 \
                    travelgo-backend
                '''
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    sleep 5
                    docker ps
                    curl -f http://localhost:4000
                '''
            }
        }
    }
}
