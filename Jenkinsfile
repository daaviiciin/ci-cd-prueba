pipeline {
    agent any

    tools {
        sonarScanner 'SonarScanner' // Usa el nombre definido en Jenkins > Global Tool Configuration
    }

    environment {
        SCANNER_HOME = tool 'SonarScanner'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials-id', url: 'https://github.com/daaviiciin/ci-cd-prueba.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube-Local') {
                        sh '''
                            ${SCANNER_HOME}/bin/sonar-scanner \
                            -Dsonar.projectKey=ci-cd-prueba \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONAR_TOKEN}
                        '''
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "El análisis ha fallado. Revisa los logs."
        }
    }
}







