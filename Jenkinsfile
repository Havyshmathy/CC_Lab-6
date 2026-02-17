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
                docker rm -f backend1 backend2 nginx || true

                docker run -d --name backend1 backend-app
                docker run -d --name backend2 backend-app
                '''
            }
        }

        stage('Run NGINX Load Balancer') {
            steps {
                sh '''
                docker run -d --name nginx \
                --link backend1 \
                --link backend2 \
                -p 8081:80 \
                -v $(pwd)/nginx/nginx.conf:/etc/nginx/nginx.conf \
                nginx
                '''
            }
        }
    }
}
