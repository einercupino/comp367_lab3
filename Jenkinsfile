pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'einercupino/lab3:latest'
        DOCKER_HUB_CREDENTIALS_ID = 'einercupino'
    }

    tools {
        maven 'Maven'
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
                tool 'Maven'
                bat 'mvn test jacoco:report'
            }

            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/classes', sourcePattern: '**/src/main/java'
                }
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
    success {
        echo 'The pipeline has completed successfully.'
    }
    failure {
        echo 'The pipeline failed.'
    }
    always {
        echo 'This will always run regardless of the build result.'
    }
    unstable {
        echo 'The build is unstable.'
    }
    changed {
        echo 'The build result has changed.'
    }
}

}
