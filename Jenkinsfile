pipeline {
    agent any

    environment {
        IMAGE_NAME = "sreevastava09/app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Sreev10/ArgoCD.git'
            }
        }

        stage('Build Maven') {
            steps {
                dir('app') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('app') {
                    sh 'docker build -t $IMAGE_NAME:$TAG .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'Sreevastava09',
                    passwordVariable: 'Vastava@007'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME:$TAG'
                }
            }
        }

        stage('Update K8S YAML') {
            steps {
                sh """
                sed -i 's#image:.*#image: $IMAGE_NAME:$TAG#g' k8s/deployment.yaml
                """
            }
        }
    }
}
