pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t flask-build -f Dockerfile.build .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker build -t flask-test -f Dockerfile.test .'
                sh 'docker run --rm flask-test'
            }
        }
    }
}
