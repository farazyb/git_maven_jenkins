pipeline {
    agent any

    tools {
        maven 'maven'
    }
    environment {
        PROJECT_NAME = sh(script: 'mvn help:evaluate -Dexpression=project.artifactId -q -DforceStdout', returnStdout: true).trim()
        PROJECT_VERSION = sh(script: 'mvn help:evaluate -Dexpression=project.version -q -DforceStdout', returnStdout: true).trim()
        COMMIT_ID = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url:'https://github.com/farazyb/git_maven_jenkins.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
stage('Rename JAR') {
            steps {
                script {
                    def jarFile = "${env.PROJECT_NAME}-${env.PROJECT_VERSION}.jar"
                    def newJarFile = "${env.PROJECT_NAME}-${env.PROJECT_VERSION}-${env.COMMIT_ID}.jar"
                    sh "mv target/${jarFile} target/${newJarFile}"
                    env.JAR_FILE = newJarFile
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