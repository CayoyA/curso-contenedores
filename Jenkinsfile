pipeline {
    agent none
    environment {
        IMAGE_NAME = 'curso-contenedores'
        DH_REPO = 'cayoya/${env.IMAGE_NAME}'
        GH_REPO = 'ghcr.io/cayoya/${env.IMAGE_NAME}'
    }
    stages {
        stage('CI de nuestra aplicacion de contenedores') {
            agent{
                docker {
                     image 'ghcr.io/pnpm/pnpm:latest'
                     label 'docker'
                }
            }
            stages{
                stage('CI - Configuracion de pnpm y node') {
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
                stage('CI - Ejecucion de build') {
                    steps {
                        sh '''
                        pnpm build
                        '''
                    }
                } 
                stage('CD - Empaquetado y distribucion') {
                    agent { label 'docker' }
                    steps {
                        sh '''
                            docker build -t ${env.IMAGE_NAME} .
                            docker tag ${env.IMAGE_NAME} ${env.DH_REPO}
                            docker tag ${env.IMAGE_NAME} ${env.GH_REPO}
                        '''
                    }
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credential') {
                            sh "docker push ${env.DH_REPO}"
                        }
                        docker.withRegistry('https://ghcr.io', 'github-credential') {
                            sh "docker push ${env.GH_REPO}"
                        }
                    }
                }
            }           
        }
    }
}