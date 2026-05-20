pipeline {
    agent {
        label 'container'
    }
        stages{
        stage('Primer paso pipeline') {
            steps {
                sh 'echo "saludos desde el terminal"'
            }
        }
        stage('Segundo paso a paso pipeline') {
            steps {
                sh "node --version"
           }
        }
        stage('Tercer paso pipeline') {
            steps {
                sh 'docker ps'
            }
        }
    }
}