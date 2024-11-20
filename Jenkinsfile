pipeline {
    agent any
     environment {
        // Define credentials ID for Git
        TESTCONTAINERS_RYUK_DISABLED = 'true'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Use Maven to build the project
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Execute SonarQube analysis with a SonarQube environment
                withSonarQubeEnv('SonarQube') {
                    script {
   
                        sh 'mvn clean verify sonar:sonar'
                    }
                }
            }
        }

    }

    post {
        success {
            echo 'Build successful'
        }
        failure {
            echo 'Build failed'
        }
        always {
            cleanWs()  // Clean workspace after build
        }
    }
}
