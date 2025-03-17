pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checkout Github repository branches'
                checkout scmGit(branches: [[name: '*/main'], [name: '*/ft-setup']], extensions: [], userRemoteConfigs: [[url: 'git@github.com:DKallans/jenkins-trial.git']])
            }
        }
        stage('Build') {
            steps {
                echo 'Build App'
                bat 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing App'
            }
        }

        stage('Archive') {
            steps {
                echo 'Archiving Artifact'
                archiveArtifacts artifacts: 'target/*.jar, target/*.war'
            }
        }

        stage('Saving to Local') {
            steps {
                echo 'Saving jar to local directory'

                script {
                    // Define the target folder where you want to deploy the .jar file
                    def targetFolder = '"C:\\Users\\allan\\OneDrive\\Desktop\\Jenkins"' // Replace with your local folder path

                    // Create the target folder if it doesn't exist
                    bat "if not exist \"${targetFolder}\" mkdir \"${targetFolder}\""

                    // Move the .jar file to the specified folder
                    bat "copy target\\*.jar \"${targetFolder}\\\""
                }
            }
        }
    }
    
    post {
        always {
            emailext body: 'Summary', subject: 'PipelineStatus', to: 'devduku@gmail.com'
        }
    }
    
}
