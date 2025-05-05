pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQube'  // Asumiendo que ya has configurado SonarQube en Jenkins
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    // Cambia la carpeta al lugar donde está el código Maven
                    dir('Gestor_Incidencias') {
                        // Ejecuta Maven desde la carpeta donde se encuentra el código
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Asegúrate de tener el token de SonarQube en las credenciales de Jenkins
                    withSonarQubeEnv('SonarQube') {
                        dir('Gestor_Incidencias') {
                            sh 'mvn sonar:sonar'
                        }
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Espera el análisis de SonarQube
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}












