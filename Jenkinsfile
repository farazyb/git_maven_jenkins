pipeline {
    agent any
     tools {
      maven 'maven'
      jdk 'JAVA_HOME'
    }

    environment {
        PROJECT_NAME = ""
        PROJECT_VERSION = ""
        COMMIT_ID = ""
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/farazyb/git_maven_jenkins.git'
            }
        }

        stage('Set Project Details') {
            steps {
                script {
                    PROJECT_NAME = sh(script: 'mvn help:evaluate -Dexpression=project.artifactId -q -DforceStdout', returnStdout: true).trim()
                    PROJECT_VERSION = sh(script: 'mvn help:evaluate -Dexpression=project.version -q -DforceStdout', returnStdout: true).trim()
                    COMMIT_ID = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
                }
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Rename JAR') {
            steps {
                script {
                    def jarFile = "${PROJECT_NAME}-${PROJECT_VERSION}.jar"
                    def newJarFile = "${PROJECT_NAME}-${PROJECT_VERSION}-${COMMIT_ID}.jar"
                    sh "mv target/${jarFile} target/${newJarFile}"
                    env.JAR_FILE = newJarFile
                }
            }
        }
    }

    post {
        success {
            echo 'Build and tests were successful!'
            echo "Deployed JAR: ${env.JAR_FILE}"
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}