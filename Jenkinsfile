pipeline {
    agent any
    stages {
        stage('Docker sanity') {
            steps {
                sh 'which docker'
                sh 'docker version'
                sh 'docker run --rm hello-world'
            }
        }
    }
}