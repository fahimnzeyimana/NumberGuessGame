pipeline {
    agent any

        stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: 'sqp_b860c1c795ac5fd5e6e589e6f379751f29e91ef6'
                
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
