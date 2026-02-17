pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app ./backend'
            }
        }

        stage('Run Backend Containers') {
            steps {
                sh '''
                # Remove old containers if they exist
                docker rm -f backend1 backend2 || true

                # Run backend containers
                docker run -d --name backend1 -p 5001:5000 backend-app
                docker run -d --name backend2 -p 5002:5000 backend-app
                '''
            }
        }

        stage('Build Custom NGINX Image') {
            steps {
                sh 'docker build -t nginx-lb ./nginx'
            }
        }

        stage('Run NGINX Load Balancer') {
            steps {
                sh '''
                # Remove old NGINX container if it exists
                docker rm -f nginx || true

                # Run NGINX load balancer
                docker run -d --name nginx \
                    --link backend1 \
                    --link backend2 \
                    -p 8081:80 \
                    nginx-lb
                '''
            }
        }
    }
}

