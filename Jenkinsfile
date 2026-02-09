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
                      npm run build
                      ls -la
                    "
                '''
            }
        }
        stage('Test'){
            steps{
               sh '''
               docker run --rm \
               -v "$PWD:/app \
               -w /app \
               sh -c "
                 test -f 'build/index.html'
                 npm test
               "
               '''
            }
        }
    }
}