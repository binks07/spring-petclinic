pipeline {
    agent any
    tools {
        maven 'Maven' // Use the defined Maven tool configuration in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                // Give permission to run the Maven wrapper and build the application
                sh '''
                    chmod +x mvnw
                    ./mvnw clean package -DskipTests
                '''
            }
        }
        stage('Execute') {
            steps {
                // Run the application on port 8085
                sh '''
                    cd target
                    nohup java -jar spring-petclinic-*.jar --server.port=8085 > petclinic.log 2>&1 &
                    sleep 30
                '''
            }
        }
    }
    post {
        success {
            echo 'Build and execution completed successfully! Application is running at http://localhost:8085'
        }
        failure {
            echo 'Build or execution failed!'
        }
    }
}
