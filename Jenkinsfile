pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle build'
        sh 'gradle javadoc'
        sh 'gradle archive'
      }
    }
    stage('Mail Notification') {
      steps {
        mail(subject: 'Build Done', to: 'ff_abdiche@esi.dz', body: 'the Build has done')
      }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
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