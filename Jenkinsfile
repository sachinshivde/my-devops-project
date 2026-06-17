pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_USERNAME/my-devops-project.git'
            }
        }

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t devops-app .'
            }
        }

        stage('Push ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com

                docker tag devops-app:latest ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/devops-app:latest

                docker push ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com/devops-app:latest
                '''
            }
        }

        stage('Deploy EKS') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
