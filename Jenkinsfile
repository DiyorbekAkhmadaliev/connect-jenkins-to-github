pipeline {
    agent any
       environment {
            JAVA_HOME = "/usr/lib/jvm/java-1.17.0-openjdk-amd64"  // replace with your JDK path
        }

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
                withSonarQubeEnv('Sonar') {
                    sh './mvnw sonar:sonar'
                }
            }
        }

        stage("Quality Gate"){
            steps{
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Deployment') {
            steps {
                echo 'Deploying to Tomcat...'
                deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://localhost:6060/')], contextPath: '', war: '**/*.war'
            }
        }



    }
}