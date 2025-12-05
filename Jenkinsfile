pipeline {
    agent any

    tools {
        maven "maven-3"
        jdk "jdk-17"
    }

    environment {
        SONARQUBE = credentials('jenkins-token') // optional if using token creds
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ARUNBABU99/sample-java-app.git', branch: 'main'
            }
        }

        stage('Build & Test') {
            steps {
                sh "mvn clean verify"
            }
        }

        stage('Sonar Scan') {
            steps {
                withSonarQubeEnv('Local-Sonar') {
                    sh "mvn sonar:sonar"
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
