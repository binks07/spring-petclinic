pipeline {
    agent any
    environment {
        MAVEN_HOME = tool name: 'Maven', type: 'maven'
        PATH = "${MAVEN_HOME}/bin:${env.PATH}" // Add Maven to the PATH
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git url: 'https://github.com/binks07/spring-petclinic.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                // Use Maven to clean and build the project
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                // Run the unit tests
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                // Create the executable JAR file
                sh 'mvn package'
            }
        }
        stage('Run Application') {
            steps {
                // Run the application on a different port to avoid conflicts
                sh 'nohup java -jar target/*.jar --server.port=8085 &'
            }
        }
    }
    post {
        success {
            echo 'Build completed successfully! Application is running at http://localhost:9090'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
