pipeline {
    agent any

    environment {
        
        SONARQUBE_SERVER = 'SonarQube-Local'
        
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials-id', url: 'https://github.com/daaviiciin/ci-cd-prueba.git', branch: 'master'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=ci-cd-prueba \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }

    post {
        failure {
            echo "El análisis ha fallado. Revisa los logs."
        }
        success {
            echo "Análisis completado exitosamente."
        }
    }
}






