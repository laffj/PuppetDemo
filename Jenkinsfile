pipeline {
  agent any
  stages {
    stage ('Building') {
      parallel {
        stage('Build Java 7') {
          steps {
            sh 'env > java7.txt'
          }
          post{
            success{
              archiveArtifacts 'java7.txt'
              stash(name: 'Java 7', includes: '**')
              }
            }
        }
        stage('Build Java 8') {
          steps {
            sh 'env > java8.txt'
          }
          post{
            success{
            archiveArtifacts 'java8.*'
            stash(name: 'Java 8', includes: '**')
          }
        }
      }
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
            echo 'Testing build on Java 72'
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
            body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' - has completed staging!!",
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
