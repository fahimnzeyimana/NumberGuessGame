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
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                // Use the injected token variable from withCredentials instead of hardcoding
                withCredentials([string(credentialsId: 'sona', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        mvn sonar:sonar \
                         -Dsonar.projectKey=NumberGuessGame \
                         -Dsonar.projectName='NumberGuessGame' \
                         -Dsonar.host.url=http://34.205.127.228:9000 \
                         -Dsonar.login=${SONAR_TOKEN}
                    '''
                }
            }
        }

        stage('Nexus Upload') {
            steps {
                // Use the injected credentials from withCredentials
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USER')]) {
                    // Corrected the double slash in the URL
                    sh 'mvn deploy -DaltDeploymentRepository=nexus::default::http://18.233.169.83:8081/repository/maven-releases/ -Dmaven.username=${NEXUS_USER} -Dmaven.password=${NEXUS_PASSWORD}'
                }
            }
        }

        stage('Tomcat Deploy') {
            steps {
                sh 'mvn tomcat7:redeploy'
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
