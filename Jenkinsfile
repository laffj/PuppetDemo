pipeline {
    agent { node{label 'centos'}}
    stages {
        stage('build') {
            steps {
                sh 'echo Email From OKE'
          emailext (
                to: 'jlaffey@cloudbees.com',
                subject: 'Test from Jenkins',
                body: 'details, details',
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
              )
            }
        }
    }
}
