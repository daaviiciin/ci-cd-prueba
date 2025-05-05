pipeline {
    agent any

    environment {
        SONAR_TOKEN = 'sonarqube1'  // Definición local del token de SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube-Local') {  // Asegúrate de que 'SonarQube-Local' sea el nombre de tu configuración de SonarQube
                        sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '¡Análisis de SonarQube completado con éxito!'
        }

        failure {
            echo '¡El análisis de SonarQube falló! Revisa los logs para más detalles.'
        }
    }
}









