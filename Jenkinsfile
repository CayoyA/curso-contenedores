pipeline {
    agent none
    stages {
        stage('CI de nuestra aplicacion de contenedores') {
            agent{
                docker {
                     image 'ghcr.io/pnpm/pnpm:latest'
                     label 'docker'
                }
            }
            stages{
                stage ('CI - Configuracion de pnpm y node') {
                   steps {
                        sh '''
                        pnpm runtime set node 24 --global
                        pnpm --version
                        '''
                    }
                }
                stage('CI - Instalacion de dependencias') {
                    steps {
                        sh '''
                        pnpm install
                        '''
                    }
                }
                stage('CI - Revision de linter'){
                    steps {
                        sh '''
                        pnpm lint
                        '''
                    }
                }
                stage ('CI - Ejecucion de build') {
                    steps {
                        sh '''
                            pnpm build
                        '''
                    }
                }
            }    
                stage('CD - Empaquetado y distribucion') {
                    agent {
                        docker {
                            image 'docker:latest'
                            label 'docker'
                            args '-v /var/run/docker.sock:/var/run/docker.sock'
                        }
                    }
                    steps {
                        sh '''
                        docker build -t mi-aplicacion:latest .
                        '''
                    }
        }
    }
}