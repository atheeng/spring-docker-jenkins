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
                 sh 'mvnw.cmd clean package -DskipTests'
            }
        }

        stage('Stop Old App') {
            steps {
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
