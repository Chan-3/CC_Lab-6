pipeline {
    agent any

    stages {

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backend Containers') {
            steps {
                sh '''
                docker network create cc_lab6_net || true

                docker rm -f backend1 backend2 || true

                docker run -d --name backend1 --network cc_lab6_net backend-app
                docker run -d --name backend2 --network cc_lab6_net backend-app

                sleep 3
                '''
            }
        }

        stage('Deploy NGINX') {
            steps {
                sh '''
                docker rm -f nginx-lb || true

                docker run -d --name nginx-lb --network cc_lab6_net -p 80:80 nginx

                sleep 2

                docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
}
