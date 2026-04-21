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
                sh 'mkdir -p reports'
                sh 'docker run --name flask-test-container -v $(pwd)/reports:/app/reports flask-test || true'
            }
        }
        stage('Report') {
            steps {
                junit 'reports/*.xml'
            }
        }
        stage('Publish - Artifact') {
            steps {
                sh 'docker run --rm -v $(pwd)/dist:/app/dist flask-build python -m build'
                archiveArtifacts artifacts: 'dist/*.whl', fingerprint: true
            }
        }
    }
    post {
        always {
            sh 'docker rm -f flask-test-container || true'
        }
        success {
            echo 'Sukces Wszystkie testy przeszły pomyślnie.'
        }
        failure {
            echo 'Uwaga Pipeline zakończony błędem.'
        }
    }
}
