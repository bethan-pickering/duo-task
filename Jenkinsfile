pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t bethanpickering/duo-jenk:latest -t bethanpickering/duo-jenk:v${BUILD_NUMBER} .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push bethanpickering/duo-jenk:latest
                docker push bethanpickering/duo-jenk:v${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh'''
                kubectl apply -f ./kubernetes
                kubectl set image deployment/flask-deployment flask-container=bethanpickering/duo-jenk:v${BUILD_NUMBER}
                '''
            }
        }
    }
}
