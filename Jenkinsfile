pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven-3.8'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Clean') {
            steps {
                echo 'Cleaning up previous build artifacts...'
                sh 'rm -rf target'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running the Petclinic application on port 8085...'
                sh 'nohup java -jar target/spring-petclinic-3.4.0-SNAPSHOT.jar --server.port=8085 > petclinic.log 2>&1 &'
            }
        }
    }
    post {
        always {
            echo 'Build, analysis, or execution completed!'
        }
        success {
            echo 'The build was successful!'
        }
        failure {
            echo 'The build failed!'
        }
    }
}
