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

            withSonarQubeEnv 'sonarqube' {
                               sh 'mvn clean package sonar:sonar'
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