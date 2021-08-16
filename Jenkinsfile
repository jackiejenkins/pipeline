pipeline {
    agent any

    stages {
        stage('clone git') {
            steps {
                echo 'git cloned..'
                git branch: 'main', url: 'https://github.com/jackiejenkins/pipeline.git'
            }
        }
        stage('maven build') {
            steps {
                echo 'aml build starting...'
                sh '''cd aml
                cat web.txt
                sed -i \'s/uml/aml/g\' web.txt
                cat web.txt'''
                
                echo 'ums build starting...'
                sh '''cd ums
                cat web.txt
                sed -i \'s/uml/ums/g\' web.txt
                cat web.txt'''

                echo 'cms build starting...'
                sh '''cd cms
                cat web.txt
                sed -i \'s/uml/cms/g\' web.txt
                cat web.txt'''
            }
           
        }
        stage('Docker build') {
            steps {
                echo 'Docker build starting...'
                sh '''cd aml
                docker build -t robydocker/aml:1.0 .'''

                echo 'Docker build starting...'
                sh '''cd ums
                docker build -t robydocker/ums:1.0 .'''

                echo 'Docker build starting...'
                sh '''cd cms
                docker build -t robydocker/cms:1.0 .'''
                
            }
            
        }
        stage('docker push') {
            steps {
                echo 'pushing aml image to docker hub....'
                withCredentials([string(credentialsId: 'DOCKER_CRED', variable: 'DOCKER_CRED')]) {
                    sh 'docker login -u robydocker -p ${DOCKER_CRED}'
                }
                sh 'docker push robydocker/aml:1.0 '
                sh 'docker push robydocker/ums:1.0 '
                sh 'docker push robydocker/cms:1.0 '
            }
            
        }
        stage('K8s Deploy') {
            steps {
                echo 'deploying aml to k8s'
                sh 'kubectl apply -f aml-deployment.yaml'

                echo 'deploying ums to k8s'
                sh 'kubectl apply -f ums-deployment.yaml'

                echo 'deploying cms to k8s'
                sh 'kubectl apply -f cms-deployment.yaml'
            }
            
        }
    }
}
