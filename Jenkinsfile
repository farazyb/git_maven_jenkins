pipeline {
    agent any

    tools {
        maven 'Maven 3.6.3'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/farazyb/jenkins-maven-project.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }

    }

    post {
        success {
            echo 'Build and tests were successful!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}