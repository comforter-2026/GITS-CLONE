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
                npm ci --cache /tmp/ .npm-cache
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
                     CI=npm test
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
                
                sh '''
                   curl -fsSL https://raw.githubusercontent.com/render-oss/cli/refs/heads/main/bin/install.sh | sh
                   Render --version
                '''
                 
            }
        }
        }

    post {
        always {
            junit 'test-result/junit.xml'
        }
        success {
            echo 'pipeline compleed - app deployed to Render!'
        }
        failure {
            echo 'pipeline failed - deployment skipped'
        }
}
}
