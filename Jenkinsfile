pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID='aff2746e-65e5-443a-a379-dee099e711aa'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        NETLIFY_DISABLE_STATUS = true
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
    }
     stage('Deploy') {
            steps {
            sh '''
              docker run --rm \
              -v "$PWD:/app" \
              -w /app \
              -e NETLIFY_AUTH_TOKEN=$NETLIFY_AUTH_TOKEN \
              -e NETLIFY_SITE_ID=$NETLIFY_SITE_ID \
              -e NETLIFY_DISABLE_STATUS=true \
                 node:18-alpine \
                sh -c "
                  npm install netlify-cli@20.1.1 &&
                  node_modules/.bin/netlify --version &&
                  echo 'deploying to production: '$NETLIFY_SITE_ID &&
                  node_modules/.bin/netlify deploy \
                  --prod \
                  --dir=build \
                  --site=$NETLIFY_SITE_ID \
                  --auth=$NETLIFY_AUTH_TOKEN
    "
'''
            }
        }
}
    // post {
    //     always {
    //         echo "Pipeline finished.."
    //     }
    // }
}