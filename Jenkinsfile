pipeline {
    agent any

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
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t nodemain:v1.0 .'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {

                    if (env.BRANCH_NAME == 'main') {

                        sh '''
                        docker stop main || true
                        docker rm main || true

                        docker run -d \
                        --name main \
                        --expose 3000 \
                        -p 3000:3000 \
                        nodemain:v1.0
                        '''

                    } else if (env.BRANCH_NAME == 'dev') {

                        sh '''
                        docker stop dev || true
                        docker rm dev || true

                        docker run -d \
                        --name dev \
                        --expose 3001 \
                        -p 3001:3000 \
                        nodedev:v1.0
                        '''
                    }
                }
            }
        }
    }
}
