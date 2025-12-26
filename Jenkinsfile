pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = "iamabi"
        IMAGE_NAME = "demo_cicd"
        IMAGE_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/amAbishek/basic_cicd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                docker run --rm $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG \
                python -m unittest test_app.py
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDENTIALS_ID,
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh '''
                docker push $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout'
            sh 'docker system prune -f'
        }
        success {
            echo 'Docker image pushed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
