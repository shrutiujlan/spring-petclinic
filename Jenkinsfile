pipeline {
    agent any
    //Java and Maven configurations in the Jenkins Global Tool Configuration
    tools {
        maven 'Maven'
        jdk 'JDK17'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Using Maven to build the project
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Executing SonarQube analysis with a SonarQube environment
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
    }
}
