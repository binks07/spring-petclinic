pipeline {
    agent any
    tools {
        maven 'Maven' // Use the defined Maven tool configuration in Jenkins
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
            echo 'Build,and execution completed successfully! Application is running at http://localhost:8085'
        }
        failure {
            echo 'Build, analysis, or execution failed!'
        }
    }
}
