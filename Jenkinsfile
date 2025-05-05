pipeline {
    agent any

    tools {
        // Asegúrate de que el nombre de la herramienta Maven coincida con el configurado en Jenkins
        maven 'Maven3'
    }

    environment {
        // Define cualquier variable de entorno, como el token de SonarQube, que has configurado en Jenkins
        SONAR_TOKEN = credentials('sonarqube')
        SONARQUBE_PROJECT_KEY = 'Gestor_Incidencias'  // Define el nombre del proyecto en SonarQube
        SONARQUBE_PROJECT_NAME = 'Gestor Incidencias'  // El nombre que se verá en SonarQube
        SONARQUBE_URL = 'http://localhost:9000'  // URL de tu servidor de SonarQube
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
                    // Usamos la herramienta de Maven para obtener la ruta correcta
                    def mvn = tool name: 'Maven', type: 'ToolType'
                    echo "Usando Maven desde: ${mvn}"

                    // Ejecutar el análisis de SonarQube con las configuraciones de proyecto y SonarQube
                    withSonarQubeEnv('SonarQube-Local') {
                        sh """
                            ${mvn}/bin/mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} \
                            -Dsonar.projectName=${SONARQUBE_PROJECT_NAME} \
                            -Dsonar.host.url=${SONARQUBE_URL} \
                            -Dsonar.login=${SONAR_TOKEN}
                        """
                    }
                }
            }
        }

        // Etapas opcionales de postacciones o limpieza
        stage('Post Actions') {
            steps {
                cleanWs()  // Limpia el espacio de trabajo de Jenkins después de la ejecución
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














