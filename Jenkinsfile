pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/daaviiciin/ci-cd-prueba.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh """
                        sonar-scanner \
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

