pipeline {
    agent any

    tools {
        // Cambia 'Maven' por el nombre exacto de la instalación que quieres usar
        maven 'Maven'  // Usa 'Maven' si esa es la instalación que quieres utilizar
    }

    environment {
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
                    // Usamos la herramienta Maven correctamente
                    def mvn = tool name: 'Maven', type: 'ToolType'  // Usa el nombre correcto aquí
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

















