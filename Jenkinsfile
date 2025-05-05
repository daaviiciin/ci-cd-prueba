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
                //     sh 'mvn clean install'
                // }
            }
        }

        stage('Tests Backend') {
            steps {
                echo 'Ejecutando tests backend...'
                // dir('Back-End') {
                //     sh 'mvn test'
                // }
            }
        }

        stage('Empaquetar Backend') {
            steps {
                echo 'Empaquetando backend...'
                // dir('Back-End') {
                //     sh 'mvn package -DskipTests'
                // }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('Gestor_Incidencias/Back-End') {
                    withSonarQubeEnv('SonarQube-Local') {
                        sh "mvn clean verify sonar:sonar -Dsonar.login=$SONAR_TOKEN"
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
                dir('Back-End') {
                    echo 'Desplegando backend en desarrollo...'
                }
                dir('Front-End') {
                    echo 'Desplegando frontend en desarrollo...'
                }
            }
        }

        stage('Desplegar en Producción') {
            when {
                branch 'main'
            }
            steps {
                echo 'Desplegando en producción...'
                dir('Back-End') {
                    echo 'Desplegando backend en producción...'
                }
                dir('Front-End') {
                    echo 'Desplegando frontend en producción...'
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
            echo 'Limpiando workspace...'
            cleanWs()
        }
    }
}



















