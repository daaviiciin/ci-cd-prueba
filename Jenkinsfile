pipeline {
    agent any

    tools {
        sonarRunner 'SonarScanner'  // <- Aquí el nombre que diste en "Global Tool Configuration"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Local') {
                    sh """
                        ${tool 'SonarScanner'}/bin/sonar-scanner \
                          -Dsonar.projectKey=ci-cd-prueba \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://localhost:9000 \
                          -Dsonar.login=__TOKEN__
                    """
                }
            }
        }
    }
}



