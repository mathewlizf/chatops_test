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
                sh 'docker build -t chatops-test:${BUILD_NUMBER} .'
            }
        }
    }
}
