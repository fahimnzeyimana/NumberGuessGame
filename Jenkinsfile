pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: 'github1'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
                junit '**/target/surefire-reports/*.xml'    // Add this to publish JUnit test result
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sona', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=NumberGuessGame \
                      -Dsonar.projectName='NumberGuessGame' \
                      -Dsonar.host.url=http://34.205.127.228:9000 \
                      -Dsonar.token=sqp_77e4a984e186fb3d8c753dbe6faf7eeee8292ebd                    '''
                }
            }
    } // This is the missing '}' to close the stages block

    post {
        success {
            mail to: 'fahimnzeimana@gmail.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Good news! The build succeeded. Check it here: ${env.BUILD_URL}"
            echo 'Pipeline completed successfully ✅'
        }
        failure {
            mail to: 'fahimnzeimana@gmail.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "The build failed. Please check logs here: ${env.BUILD_URL}"
            echo 'Pipeline failed ❌'
        }
    }
}
