pipeline {
    agent any
    
    triggers{
        githubPush()
    }

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
                    string(credentialsId: 'NETLIFY_HOOK', variable: 'NETLIFY_HOOK'),
                    string(credentialsId: 'SERVICE_ID', variable: 'SERVICE_ID')
                ]) {

                    sh '''
                    echo "Deploying service: $SERVICE_ID"

                   curl -X POST -d {} https://api.netlify.com/build_hooks/6a0c3f732ad5bec531d9d9f6
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