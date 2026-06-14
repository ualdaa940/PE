pipeline {
    agent any

    tools {
        // Nombres exactos extraídos de tus capturas de configuración
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
                // Al estar en un entorno Linux, usamos 'sh'
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
                // Ejecuta CheckStyle y SpotBugs de manera secuencial
                sh 'mvn checkstyle:checkstyle spotbugs:spotbugs'
            }
        }
    }

    post {
        always {
            // Publica los reportes visuales en las pestañas de Jenkins
            junit '**/target/surefire-reports/*.xml'
            jacoco execPattern: '**/target/*.exec', classPattern: '**/target/classes'
            recordIssues enabledForFailure: true, tool: checkStyle()
            recordIssues enabledForFailure: true, tool: spotBugs()
        }
    }
}
