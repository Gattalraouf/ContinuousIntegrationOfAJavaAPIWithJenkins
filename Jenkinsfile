pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle build'
        sh 'gradle javadoc'
        sh 'gradle jar'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/'
      }
    }
    stage('Mail Notification') {
      steps {
          echo "testing Mail Notif"       
      }
    }
    post {
          failure {
              mail(subject: 'Jinkens Test Failed', body: 'Mail Notification of the  Integration JAVA API with Jenkins', bcc: 'fa_gattal@esi.dz ', cc: 'fa_boutemine@esi.dz')
          }
          success {
            mail(subject: 'Jinkens Test Successful', body: 'Mail Notification of the  Integration JAVA API with Jenkins', bcc: 'fa_gattal@esi.dz ', cc: 'fa_boutemine@esi.dz')
          }
    }
    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonarqube') {
              sh '/home/asta/Servers/sonar-scanner-cli-3.3.0.1492-linux/sonar-scanner-3.3.0.1492-linux/bin/sonar-scanner '
            }
            
            
              waitForQualityGate true
            }
            
        }
        stage('Test reporting') {
          steps {
            jacoco(deltaInstructionCoverage: '60')
          }
        }
      }
    }
    stage('Deployment') {
      steps {
        sh 'gradle uploadArchives'
      }
    }
    stage('Slack Notification') {
      steps {
        slackSend(message: 'Testing Jenkins')
      }
    }
  }
}
