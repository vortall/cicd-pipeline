pipeline {
    agent any

    tools {
        nodejs 'node'
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub')
    }

    stages {
        stage('Build') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }

        stage('Test') {
            steps {
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin'
            }
        }


        stage('Build and Push') {
            steps {
                script{
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t vortall/nodemain:v1.0 .'
                        sh 'docker push vortall/nodemain:v1.0'
                    }
                    
                    else {
                        sh 'docker build -t vortall/nodedev:v1.0 .'
                        sh 'docker push vortall/nodedev:v1.0'
                    }
                }
            }
        }

        stage('Trigger') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        build 'Deploy_to_main'
                    }
                    else {
                        build 'Deploy_to_dev'
                    }
                }
            }      
        }   

    post {
        always {
            sh 'docker logout'
            }
        }
    }
}