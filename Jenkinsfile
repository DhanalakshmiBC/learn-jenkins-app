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
    }
}