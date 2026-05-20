pipeline {
    agent none
    
    environment {
        IMAGE_NAME = 'curso-contenedores'
        DH_REPO    = "cayoya/${env.IMAGE_NAME}"
        GH_REPO    = "ghcr.io/cayoya/${env.IMAGE_NAME}"
    }
    
    stages {
        // --- BLOQUE 1: TODO LO QUE SE HACE CON PNPM/NODE ---
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
        } // Fin del bloque pnpm

        // --- BLOQUE 2: EMPAQUETADO DOCKER (Fuera del contenedor de pnpm) ---
        stage('CD - Empaquetado y distribucion') {
            agent { label 'docker' } // Corre directo en el host que tiene Docker instalado
            steps {
                script {
                    // En Linux usamos $IMAGE_NAME, $DH_REPO y $GH_REPO directamente
                    sh '''
                        docker build -t $IMAGE_NAME .
                        docker tag $IMAGE_NAME $DH_REPO
                        docker tag $IMAGE_NAME $GH_REPO
                    '''
                    
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