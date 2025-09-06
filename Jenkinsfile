pipeline {
    agent any

    environment {
        MVN_HOME = tool name: 'Maven', type: 'maven'
        JAVA_HOME = tool name: 'JDK17', type: 'jdk'
        PATH = "${JAVA_HOME}/bin:${MVN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git',
                    branch: 'main',
                    credentialsId: '8688c497-760e-4259-8c37-cbfe8ad065f8'
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
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh """
                    mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=com.studentapp:NumberGuessGame \
                        -Dsonar.projectName='Number Guessing Game' \
                        -Dsonar.host.url=http://44.201.108.171:9000 \
                        -Dsonar.token=sqp_d2780b5c7803d68e7fb3e3aa9ae77d80225bfd6d
                    """
                }
            }
        }

        stage('Publish to Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh """
                    mvn deploy -DskipTests \
                        -Dnexus.username=${NEXUS_USER} \
                        -Dnexus.password=${NEXUS_PASS}
                    """
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'tomcat-creds', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASS')]) {
                    sh """
                    curl -u ${TOMCAT_USER}:${TOMCAT_PASS} \
                        --upload-file target/NumberGuessGame-1.0-SNAPSHOT.war \
                        http://localhost:8081/manager/text/deploy?path=/NumberGuessGame&update=true
                    """
                }
            }
        }

        stage('Run with Jetty (Optional)') {
            steps {
                sh 'mvn jetty:run &'
                sh 'sleep 10'
                echo 'Application deployed on Jetty for testing!'
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'curl -s -o /dev/null -w "%{http_code}" http://localhost:8081/NumberGuessGame'
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
