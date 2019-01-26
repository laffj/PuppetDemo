pipeline {
    agent { node{label 'centos'}}
    stages {
        stage('build') {
            steps {
                sh 'echo Hello From OKE'
                mail to: 'jlaffey@sbcglobal.net',
                    subject: "Successfull Pipeline: ${currentBuild.fullDisplayName}",
                    body: "${env.BUILD_URL} worked fine"
            }
        }
    }
}
