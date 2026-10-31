pipeline {
    agent any

    environment {
        IMAGE_NAME = 'karthikeyan0602/jenkins-docker-demo'
        IMAGE_TAG = '1'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting Source Code'
                checkout scm
            }
        }

        stage('Docker Version') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t "$IMAGE_NAME:$IMAGE_TAG" .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PASS" | docker login \
                            --username "$DOCKER_USER" \
                            --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push "$IMAGE_NAME:$IMAGE_TAG"'
            }
        }
    }

    post {
        success {
            echo 'Docker image successfully published!'
        }

        failure {
            echo 'Pipeline Failed!'
        }

        always {
            sh 'docker logout || true'
        }
    }
}
