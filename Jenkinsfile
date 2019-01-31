pipeline {
  agent any
  stages {
    stage('Building') {
      parallel {
        stage('Build Java Merrill') {
          post {
            success {
              stash(name: 'Java 7', includes: 'java7.txt')

            }

          }
          steps {
            sh 'env > java7.txt'
          }
        }
        stage('Build Java 8') {
          post {
            success {
              stash(name: 'Java 8', includes: 'java8.txt')

            }

          }
          steps {
            sh 'env > java8.txt'
          }
        }
      }
    }
    stage('Testing') {
      parallel {
        stage('Testing Java7') {
          agent {
            node {
              label 'java7'
            }

          }
          steps {
            unstash 'Java 7'
            sh 'cat java7.txt'
          }
        }
        stage('Testing Java8') {
          agent {
            node {
              label 'java8'
            }

          }
          steps {
            unstash 'Java 8'
            sh 'cat java8.txt'
          }
        }
      }
    }
    stage('Staging Notification') {
      when {
        branch 'staging'
      }
      steps {
        emailext(to: 'jlaffey@sbcglobal.net', subject: 'Build is in Staging', body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' - has completed staging!!", recipientProviders: [[$class: 'DevelopersRecipientProvider']])
      }
    }
    stage('Deploy') {
      when {
        branch 'master'
      }
      steps {
        checkpoint 'Ready to Deploy'
        input(message: 'Is the build okay to deploy?', ok: 'Yes')
        emailext(to: 'jlaffey@sbcglobal.net', subject: 'Test from Jenkins', body: 'details, details', recipientProviders: [[$class: 'DevelopersRecipientProvider']])
      }
    }
  }
}