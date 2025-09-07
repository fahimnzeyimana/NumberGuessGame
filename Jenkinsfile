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
                sh 'mvn clean install'
                junit '**/target/surefire-reports/*.xml'   // Add this to publish JUnit test results
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sona', variable: 'SONAR_TOKEN')]) {
                    sh '''
                       mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=NumberGuessGameUpdated \
                      -Dsonar.projectName='NumberGuessGameUpdated' \
                      -Dsonar.host.url=http://44.201.108.171:9000 \
                      -Dsonar.token=sqp_bc5ce9f8b143b452307d339c14053b5e8885aca2
                    '''
                }
            }
        }

        stage('Run with Jetty') {
            steps {
                sh 'mvn jetty:run &'
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
