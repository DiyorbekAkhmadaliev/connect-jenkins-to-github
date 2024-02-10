pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                echo 'Running Tests...'
                sh './mvnw test'
            }
        }

        stage('Build') {
            steps {
                echo 'Building...'
                sh './mvnw clean install'
            }
        }
    }
}