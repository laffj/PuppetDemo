pipeline {
    agent { node{label 'centos'}}
    stages {
        stage('build') {
            steps {
                sh 'echo Email From OKE'
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
