pipeline {
    agent any

    environment {
        IMAGE_NAME = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
        HOST_PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        CONTAINER_NAME = "${env.BRANCH_NAME}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                sh './scripts/test.sh'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker run -d \
                  --name ${CONTAINER_NAME} \
                  -p ${HOST_PORT}:3000 \
                  ${IMAGE_NAME}
                '''
            }
        }
    }
}
