pipeline {
    agent any

    tools {

        jdk 'OpenJDK-21'
        maven 'Maven 3.9'
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                checkout scm
            }
        }

        stage('Compilar Proyecto') {
            steps {

                sh 'mvn clean compile'
            }
        }

        stage('Ejecutar Tests y Cobertura') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Análisis Estático') {
            steps {

                sh 'mvn checkstyle:checkstyle spotbugs:spotbugs'
            }
        }
    }

    post {
        always {

            junit '**/target/surefire-reports/*.xml'
            jacoco execPattern: '**/target/*.exec', classPattern: '**/target/classes'
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs()
        }
    }
}
