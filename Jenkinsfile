pipeline {
    agent any

    tools {
        maven 'Maven'  // Usa el nombre que configuraste para Maven en Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonarqube1')  // Asegúrate de tener configurada esta credencial en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio desde GitHub
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                // Ejecuta el comando de Maven para construir el proyecto
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Realiza el análisis con SonarQube
                script {
                    withSonarQubeEnv('SonarQube-Local') {  // El nombre de la configuración de SonarQube en Jenkins
                        sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '¡El análisis y construcción fueron exitosos!'
        }

        failure {
            echo '¡Hubo un error durante la construcción o el análisis!'
        }
    }
}










