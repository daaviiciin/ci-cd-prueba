pipeline {
    agent any

    tools {
        // Usa el nombre exacto de la instalación de Maven configurada en Jenkins
        maven 'Maven'  // Si tu instalación se llama 'Maven', utiliza 'Maven'
    }

    environment {
        SONAR_TOKEN = credentials('sonarqube') // Si estás usando SonarQube para análisis
    }

    stages {
        stage('Checkout') {
            steps {
                // Realiza el checkout del código desde GitHub
                checkout scm
            }
        }

        stage('Verify Maven Installation') {
            steps {
                script {
                    // Verifica que Maven está instalado correctamente
                    echo "Verificando instalación de Maven..."
                    def mvnPath = tool name: 'Maven', type: 'ToolType'
                    echo "Maven está instalado en: ${mvnPath}"
                    
                    // Verifica que el comando mvn esté disponible
                    sh "${mvnPath}/bin/mvn -version"
                }
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    // Ejecuta el build de Maven solo si Maven está instalado correctamente
                    echo "Ejecutando build con Maven..."
                    sh 'mvn clean install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ejecuta el análisis de SonarQube
                    withSonarQubeEnv('SonarQube-Local') {
                        sh 'mvn sonar:sonar -Dsonar.projectKey=${SONARQUBE_PROJECT_KEY} -Dsonar.projectName=${SONARQUBE_PROJECT_NAME} -Dsonar.host.url=${SONARQUBE_URL}'
                    }
                }
            }
        }

        // Etapa opcional de limpieza de espacio de trabajo
        stage('Post Actions') {
            steps {
                cleanWs()  // Limpia el espacio de trabajo de Jenkins
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


















