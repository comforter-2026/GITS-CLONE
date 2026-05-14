pipeline {
    agent any

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                ls -la
                node --version
                npm --version
                npm ci --cache /tmp/.npm-cache
                npm run build
                ls -la
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                test -f build/index.html
                CI=true npm test -- --watchAll=false
                '''
            }
        }

        stage('Deploy to Render') {

            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }

            steps {

                withCredentials([
                    string(credentialsId: 'RENDER_API_KEY', variable: 'RENDER_API_KEY'),
                    string(credentialsId: 'render-service-id', variable: 'SERVICE_ID')
                ]) {

                    sh '''
                    echo "Deploying service: $SERVICE_ID"

                    curl -X POST "https://api.render.com/v1/services/$SERVICE_ID/deploys" \
                    -H "Authorization: Bearer $RENDER_API_KEY" \
                    -H "Content-Type: application/json" \
                    -d '{"clearCache":"clear"}'
                    '''
                }
            }
        }
    }

    post {

        always {
            echo 'Pipeline completed'
        }

        success {
            echo 'Pipeline completed - app deployed to Render!'
        }

        failure {
            echo 'Pipeline failed - deployment skipped'
        }
    }
}