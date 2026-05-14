pipeline {
    agent any
    stages {
        stage('Descargar Código') {
            steps {
                echo 'Clonando el repositorio...'                
                git branch: 'desarrollo', url: 'https://github.com/y9bkh/proyecto-devsecops.git'
            }
        }
        stage('Construir Imagen (Build)') {
            steps {
                echo 'Construyendo el contenedor...'
                sh 'docker build -t mi-app-segura:latest .'
            }
        }
        stage('Análisis de Seguridad (Trivy)') {
            steps {
                echo 'Buscando vulnerabilidades CRÍTICAS...'
                // Si Trivy encuentra fallos críticos, el pipeline se detiene aquí [cite: 22, 23]
                sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --exit-code 1 --severity CRITICAL mi-app-segura:latest'
            }
        }
        stage('Despliegue en Producción (CD)') {
            steps {
                echo '¡Imagen limpia! Desplegando en el servidor...'
                // Limpieza de contenedores antiguos [cite: 54, 55]
                sh 'docker stop app-produccion || true'
                sh 'docker rm app-produccion || true'
                // Despliegue de la nueva versión [cite: 57, 58]
                sh 'docker run -d --name app-produccion mi-app-segura:latest'
            }
        }
    }
}
