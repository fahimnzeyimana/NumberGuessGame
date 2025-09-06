pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: 'sqp_9cbd540f21589280277771ea585fc1329ebcd47f'
                
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
                withSonarQubeEnv('SonarQube Server') {
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
