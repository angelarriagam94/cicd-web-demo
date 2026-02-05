pipeline {
    agent any
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],  
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/angelarriagam94/cicd-web-demo.git',
                        credentialsId: 'github-credentials'
                    ]]
                ])
            }
        }
        
        stage('Lint / Validación') {
            steps {
                sh '''
                    echo "Ejecutando validaciones..."
                    # Tus comandos de linting aquí
                '''
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    echo "Ejecutando tests..."
                    # Comandos de testing
                '''
            }
        }
        
        stage('Build Imagen (staging)') {
            steps {
                script {
                    docker.build("cicd-web-demo:staging-${env.BUILD_ID}")
                }
            }
        }
        
        stage('Deploy a Staging') {
            steps {
                sh '''
                    echo "Desplegando en staging..."
                    # Comandos de deploy a staging
                '''
            }
        }
        
        stage('Aprobación para Producción') {
            steps {
                input message: '¿Aprobar despliegue a producción?', 
                      ok: 'Aprobar'
            }
        }
        
        stage('Promover Imagen a Producción') {
            steps {
                sh '''
                    docker tag cicd-web-demo:staging-${BUILD_ID} cicd-web-demo:production
                '''
            }
        }
        
        stage('Deploy a Producción') {
            steps {
                sh '''
                    echo "Desplegando en producción..."
                    # Comandos de deploy a producción
                '''
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completado exitosamente!'
        }
        failure {
            echo 'Pipeline falló'
        }
    }
}

