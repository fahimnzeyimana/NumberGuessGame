pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: '8688c497-760e-4259-8c37-cbfe8ad065f8'
                
            }
        }

        stage('Build & Test') {
            steps {
                // Build the project and run any unit tests
                sh 'mvn clean install'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Run with Jetty') {
            steps {
                // Start Jetty server for your servlet/JSP project
                sh 'mvn jetty:run &'
                
                // Optional: Wait a few seconds to ensure server starts
                sh 'sleep 10'
                
                echo 'Application deployed on Jetty!'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully ✅'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
