pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle build'
        sh 'gradle jar'
        sh 'gradle javadoc'
        archiveArtifacts(onlyIfSuccessful: true, artifacts: 'build/libs/*.jar')
      }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            script {
          // requires SonarQube Scanner 2.8+
          scannerHome = tool 'SonarQubeScanner'
           }
            withSonarQubeEnv('sonarqube') {
              sh 'C:\\\\sonarqube\\\\bin\\\\sonar-scanner'
            }

            waitForQualityGate true
          }
        }
        stage('Test Reporting') {
          steps {
            sh 'gradle test'
            jacoco(buildOverBuild: true)
          }
        }
      }
    }
    stage('Deployement') {
      steps {
        sh 'gradle uploadArchives'
      }
    }
    stage('Slack Notifcations') {
      steps {
        slackSend()
      }
    }
  }
}
