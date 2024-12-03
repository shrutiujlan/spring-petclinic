pipeline {
    agent any
    //Java and Maven configurations in the Jenkins Global Tool Configuration
    tools {
        maven 'Maven'
        jdk 'JDK17'
    }
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'  // Disable SSH host key checking for testing
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

        stage('Deploy') {
            steps {
                script {
                    // Run the Ansible playbook to deploy the application
                    sh 'ansible-playbook deploy.yml -i /etc/ansible/hosts'  
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

