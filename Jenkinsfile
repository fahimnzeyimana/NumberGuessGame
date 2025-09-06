pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: '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'
                
            }
        }

        stage('Build & Test') {
            steps {
                // Build the project and run any unit tests
                sh 'mvn clean install'
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
