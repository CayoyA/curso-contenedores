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
                stage('CI -Instalacion de dependencias') {
                    steps {
                        sh '''
                        pnpm install
                        '''
                    }
                }
                stage('CI - Revision de Linter'){
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
        }
    }
}