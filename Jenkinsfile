pipeline {
    agent none
    
    environment {
        // Definimos los textos fijos para que Jenkins no intente interpolar en el arranque
        IMAGE_NAME = 'curso-contenedores'
        DH_REPO    = 'cayoya/curso-contenedores'
        GH_REPO    = 'ghcr.io/cayoya/curso-contenedores'
    }
    
    stages {
        // --- BLOQUE 1: CI (PNPM / NODE) ---
        stage('CI de nuestra aplicacion de contenedores') {
            agent {
                docker {
                     image 'ghcr.io/pnpm/pnpm:latest'
                     label 'docker'
                }
            }
            stages {
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
            }
        } // Fin del bloque CI

        // --- BLOQUE 2: CD (EMPAQUETADO DOCKER EN EL HOST) ---
        stage('CD - Empaquetado y distribucion') {
            agent { label 'docker' } 
            steps {
                script {
                    // En las comillas triples simples de Linux usamos el símbolo $ directo para las variables del sistema
                    sh '''
                        docker build -t $IMAGE_NAME .
                        docker tag $IMAGE_NAME $DH_REPO
                        docker tag $IMAGE_NAME $GH_REPO
                    '''
                    
                    // Aquí usamos las variables inyectadas mediante comillas dobles usando el objeto env nativo de Jenkins
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credential') {
                        sh "docker push ${env.DH_REPO}"
                    }
                    
                    docker.withRegistry('https://ghcr.io', 'github-credential') {
                        sh "docker push ${env.GH_REPO}"
                    }
                }
            }
        }
    } // Fin de todos los stages
} // Fin del pipeline