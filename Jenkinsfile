pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-portfolio-site"
        CONTAINER_NAME = "devops-portfolio-container"
        APP_URL = "http://localhost:3001"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                    sh "docker run -d -p 3001:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                }
            }
        }

        stage('Open Application Locally') {
            steps {
                script {
                    sleep 2
                    sh '''
                        export DISPLAY=:0
                        export XAUTHORITY=/home/ayush.ranjan/.Xauthority
                        JENKINS_NODE_COOKIE=dontKillMe BUILD_ID=dontKillMe setsid google-chrome --new-window http://localhost:3001 >/dev/null 2>&1 < /dev/null &
                    '''
                }
            }
        }

    }
}

