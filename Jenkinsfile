pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script{
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl create ns prod || echo "---------- Prod Namespace Already Exists ----------"
                        '''

                    } else if ( env.GIT_BRANCH == 'origin/dev'){
                        sh '''
                        kubectl create ns dev || echo "---------- Dev Namespace Already Exists ----------"
                        '''

                    } else {
                        sh 'echo "unrecogognised branch"'
                    }
                }
            }
        }

        stage('build'){
            steps{
            script{
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        docker build -t bethanpickering/duo-jenk:latest -t bethanpickering/duo-jenk:v${BUILD_NUMBER} .
                        '''

                    } else if ( env.GIT_BRANCH == 'origin/dev'){
                        sh '''
                        docker build -t bethanpickering/duo-jenk:latest -t bethanpickering/duo-jenk:v${BUILD_NUMBER} .
                        '''

                    } else {
                        sh 'echo "unrecogognised branch"'
                    }
                }
            
            }

        }

        stage('Push') {
            steps {
                script{
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        docker push bethanpickering/duo-jenk:latest
                        docker push bethanpickering/duo-jenk:v${BUILD_NUMBER}
                        '''

                    } else if ( env.GIT_BRANCH == 'origin/dev'){
                        sh '''
                        docker push bethanpickering/duo-jenk:latest
                        docker push bethanpickering/duo-jenk:v${BUILD_NUMBER}
                        '''
                    } else {
                        sh 'echo "unrecogognised branch"'
                    }
                }
                
            }
        }
        stage('Deploy') {
            steps {
                script{
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        kubectl apply -f ./kubernetes -n prod
                        kubectl set image deployment/flask-deployment flask-container=bethanpickering/duo-jenk:v${BUILD_NUMBER} -n prod
                        '''
                    } else if ( env.GIT_BRANCH == 'origin/dev'){
                        sh'''
                        kubectl apply -f ./kubernetes -n dev
                        kubectl set image deployment/flask-deployment flask-container=bethanpickering/duo-jenk:v${BUILD_NUMBER} -n prod
                        '''
                    } else {
                        sh 'echo "unrecogognised branch"'
                    }
                }                
                
            }
        }
    }
}