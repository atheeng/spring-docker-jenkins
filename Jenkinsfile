pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5' // Ensure this matches your configured Maven version in Jenkins
        jdk 'Java 17'       // Ensure Java is installed and configured in Jenkins
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/atheeng/spring-docker-jenkins.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Stop Old App') {
            steps {
                // Try to stop the running app
                sh 'pkill -f "app.jar" || true'
            }
        }

        stage('Run New JAR') {
            steps {
                sh 'nohup java -jar target/*.jar > app.log 2>&1 &'
            }
        }
    }
}
