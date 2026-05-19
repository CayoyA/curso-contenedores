pipeline {
    agent any
    stages{
        stage('Primer paso pipeline') {
            steps {
                sh 'echo "saludos desde el terminal"'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing..."'
                // Add your test steps here
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
                // Add your deploy steps here
            }
        }
    }
}