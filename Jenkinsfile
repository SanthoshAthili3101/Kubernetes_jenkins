pipeline {
    agent {
        kubernetes {
            inheritFrom 'testcheck'
            defaultContainer 'dotnet'
        }
    }

    environment {
        IMAGE_NAME = "santhoshathili1998/demo-api"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/SanthoshAthili3101/Kubernetes_jenkins.git'
            }
        }

        stage('Restore') {
            steps {
                container('dotnet') {
                    sh 'dotnet restore'
                }
            }
        }

        stage('Build') {
            steps {
                container('dotnet') {
                    sh 'dotnet build --configuration Release'
                }
            }
        }

        stage('Test') {
            steps {
                container('dotnet') {
                    sh 'dotnet test || true'
                }
            }
        }

        stage('Publish') {
            steps {
                container('dotnet') {
                    sh 'dotnet publish -c Release -o publish'
                }
            }
        }

        stage('Docker Build') {
            steps {
                container('docker') {
                    sh '''
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                container('docker') {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )]) {
                        sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $IMAGE_NAME:$IMAGE_TAG
                        '''
                    }
                }
            }
        }
    }
}