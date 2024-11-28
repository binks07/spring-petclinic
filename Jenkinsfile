pipeline {
    agent any
    tools {
        maven 'Maven' // Use the defined Maven tool configuration in Jenkins
    }
    environment {
        SONARQUBE_SERVER = 'SonarQube' // This is the name you used in SonarQube Server Configuration in Jenkins
        SONAR_SCANNER_HOME = tool 'SonarQube Scanner' // This is the name of the SonarQube Scanner in Jenkins Tool Configuration
        SONAR_AUTH_TOKEN = 'sqp_5deb1532622f5b9e700843d9aed3882edba66a5d' // SonarQube Authentication Token
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage('Clean') {
            steps {
                echo 'Cleaning up previous build artifacts...'
                sh 'rm -rf target' // Clean up the previous build's target directory
            }
        }
        stage('Build') {
            steps {
                // Build the application
                sh '''
                    chmod +x mvnw
                    ./mvnw clean package -DskipTests
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        ./mvnw sonar:sonar \
                        -Dsonar.projectKey=spring-petclinic \
                        -Dsonar.host.url=$SONAR_HOST_URL \
                        -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }
        stage('Run Application') {
            steps {
                // Run the application on a different port to avoid conflicts
                sh '''
                    cd target
                    nohup java -jar spring-petclinic-*.jar --server.port=8085 > petclinic.log 2>&1 &
                    sleep 60
                '''
            }
        }
    }
    post {
        success {
            echo 'Build, SonarQube analysis, and execution completed successfully! Application is running at http://localhost:8085'
        }
        failure {
            echo 'Build, analysis, or execution failed!'
        }
    }
}
