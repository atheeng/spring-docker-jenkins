pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/atheeng/spring-docker-jenkins.git'
            }
        }

        stage('Make mvnw executable') {
            steps {
                script {
                    // Debug: List files in the workspace
                    echo "Listing files in the workspace:"
                    sh 'ls -l /var/jenkins_home/workspace/springboot-app'

                    // Check if the environment is Unix-based and apply chmod if true
                    if (isUnix()) {
                        echo "Making mvnw executable (Unix environment)"
                        sh 'chmod +x mvnw'
                    } else {
                        echo 'No chmod required on Windows environment.'
                    }
                }
            }
        }

        stage('Build JAR') {
            steps {
                script {
                    // Debug: Check mvnw file and permissions
                    echo "Checking mvnw permissions:"
                    sh 'ls -l mvnw'

                    // Build the JAR using the appropriate command based on the environment
                    if (isUnix()) {
                        // Debug: Show mvnw script content
                        sh 'cat mvnw'

                        // Run the build process
                        echo "Building JAR using mvnw"
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
                        echo "Stopping old app (Unix environment)"
                        sh 'pkill -f "app.jar" || true'
                    } else {
                        echo "Stopping old app (Windows environment)"
                        bat 'taskkill /F /IM app.jar || echo "No app.jar process found."'
                    }
                }
            }
        }

        stage('Run New JAR') {
            steps {
                script {
                    // Run new JAR based on the OS type
                    if (isUnix()) {
                        echo "Running new JAR (Unix environment)"
                        sh 'nohup java -jar target/*.jar > app.log 2>&1 &'
                    } else {
                        echo "Running new JAR (Windows environment)"
                        bat 'start java -jar target\\*.jar'
                    }
                }
            }
        }
    }
    post {
        always {
            echo "Cleaning up the environment"
            // Additional cleanup if needed
        }
        success {
            echo "Build and deployment successful!"
        }
        failure {
            echo "Build failed, check logs for details."
        }
    }
}
