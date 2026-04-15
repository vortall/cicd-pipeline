pipeline {
    agent any

    stages {
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

        stage('Build Docker Image') {
            steps {
                script{
                    dockerImage = docker.build("vortall/ci-cd:v1.0.0")
                }
            }
        }
    }
    
}