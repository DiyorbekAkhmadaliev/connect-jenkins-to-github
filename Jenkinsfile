pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Running Tests...'
                sh './mvnw test'
            }
            post {
                success {
                    jacoco execPattern: '**/target/jacoco.exec'
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                sh './mvnw clean install'
            }
        }

        stage('SonarQube analysis') {
            steps {
                echo 'Analyzing source code...'
                withSonarQubeEnv('My SonarQube Server') {
                    sh './mvnw sonar:sonar'
                }
            }
        }
    }
}