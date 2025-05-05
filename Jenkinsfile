pipeline {
    agent any

    tools {
        // Configura el uso de Maven, asegúrate de que esté correctamente configurado en Jenkins
        maven 'Maven'
    }

    environment {
        // Define aquí cualquier variable de entorno global si es necesario
        SONAR_TOKEN = credentials('sonarqube')
    }

    stages {
        stage('SCM') {
            steps {
                // Realiza el checkout del código fuente desde el repositorio
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Usamos la herramienta de SonarQube para la configuración del entorno
                    def mvn = tool name: 'Maven', type: 'ToolType'
                    echo "Usando Maven desde: ${mvn}"

                    // Ejecutar el análisis de SonarQube con las configuraciones de proyecto y SonarQube
                    withSonarQubeEnv('SonarQube-Local') {
                        sh """
                            ${mvn}/bin/mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} \
                            -Dsonar.projectName=${SONARQUBE_PROJECT_NAME} \
                            -Dsonar.host.url=${SONARQUBE_URL}
                        """
                    }
                }
            }
        }

        // Etapas opcionales de postacciones o limpieza
        stage('Post Actions') {
            steps {
                cleanWs()  // Limpia el espacio de trabajo de Jenkins
            }
        }
    }

    post {
        // Manejo de errores si es necesario
        failure {
            echo 'Pipeline fallido.'
        }
        success {
            echo 'Pipeline completado exitosamente.'
        }
    }
}













