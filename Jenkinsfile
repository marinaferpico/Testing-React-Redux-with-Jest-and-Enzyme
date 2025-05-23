pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        CI = 'true'
    }

    triggers {
        pollSCM('H/15 * * * *')
    }

    stages {
        stage('Install Dependencies') {
            when { branch 'main' }
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            when { branch 'main' }
            steps {
                echo '✅ Ejecutando pruebas...'
                sh 'npm test -- --ci --coverage'
            }
        }

        stage('Build') {
            when { branch 'main' }
            steps {
                echo '🏗️ Construyendo la app...'
                sh 'npm run build'
            }
        }

        stage('Deploy en contenedor del curso') {
            when { branch 'main' }
            steps {
                echo '📦 Copiando archivos al contenedor del curso...'
                sh 'mkdir -p /home/alumno/Escritorio/Practica9/Testing-React-Redux-with-Jest-and-Enzyme/deploy'
                sh 'cp -r build/* /home/alumno/Escritorio/Practica9/Testing-React-Redux-with-Jest-and-Enzyme/deploy/'
            }
        }

        stage('Deploy en contenedor Docker') {
            when { branch 'main' }
            steps {
                echo '🐳 Desplegando en contenedor Docker...'

                sh '''
                    docker rm -f react-container || true
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
