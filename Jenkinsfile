pipeline {
    agent any

    environment{
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
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
     stage('Deploy') {
            steps {
            sh '''
              docker run --rm \
                    -v "$PWD:/app" \
                    -w /app \
                    node:18-alpine \
                 sh -c "
                   npm install netlify-cli@20.1.1 &&
                   node_modules/.bin/netlify --version
                    "
                '''
            }
        }
}
    }
    post {
        always {
            junit 'junit.xml'
        }
    }
}