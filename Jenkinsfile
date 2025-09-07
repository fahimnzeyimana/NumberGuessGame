pipeline {
  agent any
  environment {
    SONARQUBE_SERVER = 'SonarQube'
    MAVEN_HOME = '/usr/share/maven'
    NEXUS_URL = '4.207.122.57:8081'
    NEXUS_REPO = 'NumberGuessGame'
    NEXUS_CREDENTIALS_ID = 'nexus'
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
          sh """
            mvn clean verify sonar:sonar \
            -Dsonar.projectKey=com.studentapp:NumberGuessGame \
            -Dsonar.projectName='Number Guessing Game' \
            -Dsonar.host.url=http://44.201.108.171:9000 \
            -
          """
        }
      }
    }
    stage('Package Artifact') {
      steps {
        sh 'mvn package'
      }
    }
    stage('Publish Artifact to Nexus') {
      steps {
        nexusArtifactUploader(
          nexusVersion: 'nexus3',
          protocol: 'http',
          nexusUrl: NEXUS_URL,
          credentialsId: NEXUS_CREDENTIALS_ID,
          repository: NEXUS_REPO,
          groupId: 'com.studentapp',
          version: '1.0-SNAPSHOT',
          artifacts: [
            [artifactId: 'NumberGuessGame', classifier: '', file: 'target/NumberGuessGame-1.0-SNAPSHOT.war', type: 'war']
          ]
        )
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
