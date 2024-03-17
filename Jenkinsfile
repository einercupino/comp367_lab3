pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'einercupino/lab3:latest'
        DOCKER_HUB_CREDENTIALS_ID = 'einercupino'
    }

    stages {
        stage('Check Out') {
            steps {
                checkout scm
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.DOCKER_HUB_CREDENTIALS_ID) {
                    }
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.image(env.DOCKER_IMAGE).push()
                }
            }
        }
    }

    post {
        always {
        }
    }
}
