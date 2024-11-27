pipeline {
    agent {
        docker {
            image 'maven:3.8.4-jdk-11'
        }
    }
    environment {
        SONARQUBE_SERVER = 'SonarQube'
        SONAR_SCANNER_HOME = tool 'SonarQube Scanner'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Create JAR') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Run Application') {
            steps {
                sh 'java -jar target/*.jar &'
            }
        }
    }
    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
