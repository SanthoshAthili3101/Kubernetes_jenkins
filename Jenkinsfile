pipeline {
    agent {
        kubernetes {
            label 'testcheck'   // MUST match Pod Template label
            defaultContainer 'dotnet'
        }
    }

    stages {
        stage('Check') {
            steps {
                container('dotnet') {
                    sh 'dotnet --version'
                }
            }
        }
    }
}