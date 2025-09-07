pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: 'github'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean'
                junit '**/target/surefire-reports/*.xml'   // Add this to publish JUnit test results
            }
        }

        stage('Deploy to Nexus') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
    sh('mvn clean deploy -DskipTests -DaltDeploymentRepository=nexus::default::http://$NEXUS_USER:$NEXUS_PASS@34.207.122.57:8081/repository/NumberGuessGame/')
        }
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
