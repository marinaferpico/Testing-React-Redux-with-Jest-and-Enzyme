pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        CI = 'true'
    }

    triggers {
        pollSCM('H/15 * * * *')
    }

    when {
        branch 'main'
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo '✅ Ejecutando pruebas...'
                sh 'npm test -- --ci --coverage'
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Construyendo la app...'
                sh 'npm run build'
            }
        }

        stage('Deploy en contenedor del curso') {
            steps {
                echo '📦 Copiando archivos al contenedor del curso...'
                sh 'cp -r build/* /ruta/del/contenedor/asignatura/'  // Cambia esto según el entorno real
            }
        }

        stage('Deploy en contenedor Docker') {
            steps {
                echo '🐳 Desplegando en contenedor Docker...'

                // Asegúrate de tener un Dockerfile en tu proyecto
                sh '''
                    docker build -t my-react-app .
                    docker run -d -p 3000:80 --name react-container my-react-app
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CD completado en main.'
        }
        failure {
            echo '❌ Error durante el despliegue.'
        }
    }
}
