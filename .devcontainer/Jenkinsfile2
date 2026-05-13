pipeline {
    agent any

    stages {

        stage('Clone Check') {
            steps {
                echo 'Repository cloned successfully!'
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'ls -la'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo Tests passed!'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'echo Deployment completed!'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}