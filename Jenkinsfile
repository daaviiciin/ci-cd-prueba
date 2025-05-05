pipeline {
    agent any

    tools {
        // Usamos el nombre de la herramienta que está configurada en Jenkins
        maven 'maven3'  // Cambié 'Maven' por 'maven3' aquí
    }

    environment {
        // Define cualquier variable de entorno
        SONAR_TOKEN = credentials('sonarqube')
        SONARQUBE_PROJECT_KEY = 'Gestor_Incidencias'
        SONARQUBE_PROJECT_NAME = 'Gestor Incidencias'
        SONARQUBE_URL = 'http://localhost:9000'  // Cambia la URL si es necesario
    }

    stages {
        stage('SCM') {
            steps {
                checkout scm  // Realiza el checkout del código fuente desde el repositorio
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Usamos la herramienta de Maven
                    def mvn = tool name: 'maven3', type: 'ToolType'  // Usamos 'maven3' aquí también
                    echo "Usando Maven desde: ${mvn}"

                    // Ejecutar el análisis de SonarQube
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

        stage('Post Actions') {
            steps {
                cleanWs()  // Limpia el espacio de trabajo
            }
        }
    }

    post {
        failure {
            echo 'Pipeline fallido.'
        }
        success {
            echo 'Pipeline completado exitosamente.'
        }
    }
}















