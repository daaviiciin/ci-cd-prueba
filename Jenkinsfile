pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Local') {
                    withSonarScanner('SonarScanner') {
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
}


