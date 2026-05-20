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
                stage ('CI - Instalacion de dependencias') {
                    steps {
                        sh '''
                            pnpm runtime set node 24 --global
                            pnpm --version                    
                            pnpm install

                            echo "--- Corriendo validación de código (Linter) ---"
                            pnpm lint
                            
                            echo "--- Corriendo Pruebas Unitarias (Tests) ---"
                            pnpm test
                        '''
                    }
                } 
            }
        }
    }
}