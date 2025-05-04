pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/atheeng/spring-docker-jenkins.git'
            }
        }

        stage('Build JAR') {
            steps {
                // Ensure that the mvnw file is executable and run it
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
                // Run the newly built JAR file in the background
                sh 'nohup java -jar target/*.jar > app.log 2>&1 &'
            }
        }
    }
}
