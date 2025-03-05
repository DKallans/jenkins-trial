pipeline {
    agent any

    tools {
        maven 'MAVEN'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Build App'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'git@github.com:DKallans/jenkins-trial.git']])
                bat 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Test App'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy App'
            }
        }
    }
    
    post {
        always {
            emailext body: 'Summary', subject: 'PipelineStatus', to: 'devduku@gmail.com'
        }
    }
    
}
