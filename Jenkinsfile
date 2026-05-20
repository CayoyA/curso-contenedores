pipeline {
    agent {
        label 'wsl'
    }
    stages {
        stage('Primer paso pipeline') {
            steps {
                sh 'echo "saludos desde el terminal"'
            }
        }
        stage('Segundo paso a paso pipeline') {
            agent {
                label 'container'
            }   
            steps {
                sh "node --version"
           }
        }
        stage('Tercer paso pipeline') {
            steps {
                sh 'docker ps'
            }
        }
        stage('Cuarto paso pipeline') {
            agent {
                docker {
                    image 'node:22'
                    label 'wsl'
                }
            }   
            steps {
                sh 'node --version'
            }
        }
    }

}