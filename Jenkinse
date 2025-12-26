pipeline {
    agent any

    environment {
        IMAGE_NAME = "simple-python-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/simple-python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh '''
                docker run --rm $IMAGE_NAME python -m unittest test_app.py
                '''
            }
        }

        stage('Run Application') {
            steps {
                sh '''
                docker run --rm $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            sh 'docker system prune -f'
        }
    }
}
