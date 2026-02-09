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
            -e CI=true \
            node:18-alpine \
            sh -c "
              npm ci &&
              npm test -- --ci --reporters=default --reporters=jest-junit
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