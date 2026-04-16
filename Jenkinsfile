pipeline {
    agent any

    tools {
        nodejs 'node'
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

        stage('Build Docker Image') {
            steps {
                script{
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker build -t nodemain:v1.0 .'
                    }
                    
                    else {
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script{
                    def port = ''
                    def image = ''

                    if (env.BRANCH_NAME == 'main') {
                        port = 3000
                        image = 'nodemain:v1.0'
                    }
                    
                    else {
                        port = 3001
                        image = 'nodedev:v1.0'
                    }

                    sh "docker run -d -p ${port}:${port} ${image}"
                }
            }
        }
    }
    
}