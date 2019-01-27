pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'This is the first step of the pipeline'
        sleep 5
      }
    }
    stage('Testing') {
      parallel {
        stage('Testing') {
          steps {
            echo 'Running tests'
            sleep 25
          }
        }
        stage('Testing Java7') {
          steps {
            echo 'Testing build on Java 7'
            sleep 20
            echo 'More testing on Java 7'
          }
        }
        stage('Testing Java8') {
          steps {
            echo 'Testing build on Java 8'
            sleep 20
            echo 'Running more tests on Java 8'
          }
        }
        stage('Testing Notify Staging') {
        when {
          branch 'staging'
        }        
         steps {
          emailext (
           to: 'jlaffey@cloudbees.com',
           subject: 'Build is in Staging',
           body: '${currentBuild.fullDisplayName} - has completed staging',
           recipientProviders: [[$class: 'DevelopersRecipientProvider']]
            )
          }
        }        
      }
    }
    stage('Deploy') {
      when {
        branch 'master'
      }
      steps {
        checkpoint 'Ready to Deploy'
        input(message: 'Is the build okay to deploy?', ok: 'Yes')
        emailext (
         to: 'jlaffey@sbcglobal.net',
         subject: 'Test from Jenkins',
         body: 'details, details',
         recipientProviders: [[$class: 'DevelopersRecipientProvider']]
              )
      }
    }
  }
}
