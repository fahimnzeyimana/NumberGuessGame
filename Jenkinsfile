pipeline {
  agent any
  environment {
    SONARQUBE_SERVER = 'SonarQube'
    MAVEN_HOME = '/usr/share/maven'
  }
  
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/fahimnzeyimana/NumberGuessGame.git', branch: 'main', credentialsId: '8688c497-760e-4259-8c37-cbfe8ad065f8'
      }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn clean install'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
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

    stage('Deploy to Jetty') {
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
