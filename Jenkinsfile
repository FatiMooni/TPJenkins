pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        failure {
          mail(subject: 'Build DONE', body: 'The build has not been succesful', to: 'ff_abdiche@esi.dz')

        }

        success {
          mail(subject: 'Build DONE', body: 'The build has been succesfully done', to: 'ff_abdiche@esi.dz')

        }

      }
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
      when{
            NOT{
              changeRequest target : 'master'
            }
          }
      steps {
        sh 'gradle uploadArchives'
      }
    }
    stage('Slack Notifcations') {
      
      when{
            NOT{
              changeRequest target : 'master'
            }
          }
      steps {
        slackSend(failOnError: true, message: 'the uploading is done succefully !')
      }
    }
  }
}
