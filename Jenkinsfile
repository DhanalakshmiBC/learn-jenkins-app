pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh '''
                  docker run --rm \
                    -v "$PWD:/app" \
                    -w /app \
                    node:18-alpine \
                    sh -c "
                      node --version &&
                      npm --version &&
                      npm ci &&
                      npm run build &&
                      ls -la
                    "
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                  docker run --rm \
                    -v "$PWD:/app" \
                    -w /app \
                    node:18-alpine \
                    sh -c "
                      npm ci &&
                      test -f build/index.html &&
                      npm install --save-dev @babel/plugin-proposal-private-property-in-object &&
                      npm test
                    "
                '''
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}