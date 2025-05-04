pipeline {
    agent any
    stages {
        stage('Make mvnw executable') {
            steps {
                script {
                    // Check if the environment is Unix-based and apply chmod if true
                    if (isUnix()) {
                        sh 'chmod +x mvnw'
                    } else {
                        echo 'No chmod required on Windows environment.'
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
                    // Build the JAR using the appropriate command based on the environment
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
                    // Stop old app based on the OS type
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
                    // Run new JAR based on the OS type
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
