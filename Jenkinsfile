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
              scannerHome = tool 'SonarQubeScanner'
            }

            withSonarQubeEnv('sonarqube') {
              sh 'D:\\\\2SIL\\\\TOOLS\\\\TPsGit\\\\TP\\\\SonarAnalyse\\\\sonar-scanner-cli-3.2.0.1227-windows\\\\sonar-scanner-3.2.0.1227-windows\\\\bin\\\\sonar-scanner.bat'
            }

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
        slackSend(failOnError: true, message: 'the uploading is done succefully !')
      }
    }
  }
}