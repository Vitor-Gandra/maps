pipeline {
    agent any

    stages {
        stage('PR') {
            when {
                changeRequest()
            }
            steps {
                sh '''
                    echo This is a pr
                    echo $PWD
                '''
            }
        }
        stage('Build Master') {
            when {
                branch 'master'
            }
            steps {
                sh '''
                    echo This is a push to Master
                '''
            }
        }
        stage('Build Staging or Qa') {
            when {
                anyOf {
                    branch 'staging'
                    branch 'qa'
                }
            }
            steps {
                sh '''
                    echo This is a push to staging or qa
                '''
            }
        }
        stage('Deploy') {
            when {
                tag "WEB-*"
            }
            input {
                message "Proceed with deploy?"
                ok "Yes"
            }
            steps {
                sh '''
                    echo Deploy Tag $TAG_NAME
                    /var/pipeline/deploy.sh $TAG_NAME
                '''
            }
        }
    }
}