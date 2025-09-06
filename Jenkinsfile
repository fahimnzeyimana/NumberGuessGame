pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    environment {
        DEPLOY_USER = 'ec2-user'
        DEPLOY_HOST = 'your-tomcat-ec2-ip'
        DEPLOY_PATH = '/opt/tomcat/webapps'
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
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                       mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=com.studentapp:NumberGuessGame \
                      -Dsonar.projectName='Number Guessing Game' \
                      -Dsonar.host.url=http://44.201.108.171:9000 \
                      -Dsonar.token=sqp_d2780b5c7803d68e7fb3e3aa9ae77d80225bfd6d
                    '''
                }
            }
        }

        stage('Publish to Nexus') {
            steps {
                sh 'mvn deploy -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(['deploy-ssh-key']) {
                    sh """
                        scp -o StrictHostKeyChecking=no target/*.war \
                          ${DEPLOY_USER}@${DEPLOY_HOST}:${DEPLOY_PATH}/ROOT.war

                        ssh ${DEPLOY_USER}@${DEPLOY_HOST} "/opt/tomcat/bin/shutdown.sh || true"
                        ssh ${DEPLOY_USER}@${DEPLOY_HOST} "/opt/tomcat/bin/startup.sh"
                    """
                }
            }
        }

        stage('Run with Jetty (Optional)') {
            steps {
                sh 'mvn jetty:run &'
                sh 'sleep 10'
                echo 'Application deployed on Jetty!'
            }
        }

        stage('Smoke Test') {
            steps {
                sh 'curl -f http://${DEPLOY_HOST}:8081 || exit 1'
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
