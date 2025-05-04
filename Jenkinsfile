pipeline {
    agent any
    stages {
        stage('Make mvnw executable') {
            steps {
                script {
                    // Ensure the correct command for Windows/Linux environments
                    if (isUnix()) {
                        sh 'chmod +x mvnw is isUnix'
                    } else {
                        bat 'echo "No chmod on Windows, mvnw should already be executable"'
                    }
                }
            }
        }
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/atheeng/spring-docker-jenkins.git'
            }
        }
        stage('Build JAR') {
            steps {
                script {
                    // Ensure the correct command for Windows/Linux environments
                    if (isUnix()) {
                        sh './mvnw clean package -DskipTests'
                    } else {
                        bat 'mvnw clean package -DskipTests'
                    }
                }
            }
        }
        stage('Stop Old App') {
            steps {
                script {
                    // Linux version
                    if (isUnix()) {
                        sh 'pkill -f "app.jar" || true'
                    } else {
                        bat 'taskkill /F /IM app.jar'
                    }
                }
            }
        }
        stage('Run New JAR') {
            steps {
                script {
                    // Linux version
                    if (isUnix()) {
                        sh 'nohup java -jar target/*.jar > app.log 2>&1 &'
                    } else {
                        bat 'start java -jar target\\*.jar'
                    }
                }
            }
        }
    }
}
