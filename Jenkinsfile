pipeline {
    agent any
    stages {
        stage('Docker test') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                sh 'node --version'
            }
        }
    }
}