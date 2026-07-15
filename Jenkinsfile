pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/karinamalyuk95-crypto/cicd-pipeline.git'
            }
        }

        stage('Install dependencies') {
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
                sh 'docker build -t cicd-pipeline-app .'
            }
        }
    }
}
