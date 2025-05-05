pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarqube1')
    }
    
    tools {
        maven 'Maven'
        jdk 'Java 23'
    }

    stages {
        stage('Verificar versión de Java') {
            steps {
                sh 'java -version'
            }
        }
        
        stage('Build Backend') {
            steps {
                echo 'Compilando proyecto backend...'
                // dir('Back-End') {
                //     sh 'chmod +x ./mvnw || true'
                //     sh './mvnw clean install || mvn clean install'
                // }
            }
        }

        stage('Tests Backend') {
            steps {
                echo 'Ejecutando tests backend...'
                // dir('Back-End') {
                //     sh './mvnw test || mvn test'
                // }
            }
        }

        stage('Empaquetar Backend') {
            steps {
                echo 'Empaquetando backend...'
                // dir('Back-End') {
                //     sh './mvnw package -DskipTests || mvn package -DskipTests'
                // }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('Gestor_Incidencias/Back-End') {
                    withSonarQubeEnv('SonarQube-Local') {
                        sh 'chmod +x ./mvnw' 
                        sh "./mvnw clean verify sonar:sonar -Dsonar.login=$SONAR_TOKEN || mvn clean verify sonar:sonar -Dsonar.login=$SONAR_TOKEN"
                    }
                }
            }
        }
        stage('Test Nexus Connection') {
        steps {
        sh 'curl -v -u admin:admin http://nexus:8081/repository/maven-snapshots/'
        }
}

       stage('Publicar en Nexus') {
    steps {
        echo 'Publicando artefactos en Nexus...'
        dir('Back-End') {
            // Usar credenciales directamente en la URL
            sh '''
                mvn deploy -DskipTests \
                -DaltDeploymentRepository=nexus::default::http://admin:admin@nexus:8081/repository/maven-snapshots/
            '''
        }
    }
}

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Frontend') {
            steps {
                echo 'Compilando proyecto frontend...'
                dir('Front-End') {
                    // Para proyectos Node.js/React/Angular
                    sh '''
                        if [ -f "package.json" ]; then
                            npm install
                            npm run build
                        else
                            echo "No se encontró package.json. Omitiendo build del frontend."
                        fi
                    '''
                }
            }
        }

        stage('Desplegar en Desarrollo') {
            when {
                branch 'develop'
            }
            steps {
                echo 'Desplegando en ambiente de desarrollo...'
                // Desplegando backend
                dir('Back-End') {
                    echo 'Desplegando backend en desarrollo...'
                    // Comandos específicos para desplegar el backend
                }
                
                // Desplegando frontend
                dir('Front-End') {
                    echo 'Desplegando frontend en desarrollo...'
                    // Comandos específicos para desplegar el frontend
                }
            }
        }

        stage('Desplegar en Producción') {
            when {
                branch 'main'
            }
            steps {
                echo 'Desplegando en producción...'
                // Desplegando backend
                dir('Back-End') {
                    echo 'Desplegando backend en producción...'
                    // Comandos específicos para desplegar el backend
                }
                
                // Desplegando frontend
                dir('Front-End') {
                    echo 'Desplegando frontend en producción...'
                    // Comandos específicos para desplegar el frontend
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado con éxito!'
        }
        failure {
            echo 'El pipeline ha fallado!'
        }
        always {
            // Limpieza opcional de recursos
            echo 'Limpiando workspace...'
            cleanWs()
        }
    }
}


















