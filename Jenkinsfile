pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
    }

    environment {
        // Define email configuration and file path
        WAR_FILE = 'target/jenkins-trial-1.0-SNAPSHOT.jar'  // Adjust this path based on your project's setup
        EMAIL_RECIPIENT = 'devduku@gmail.com'  // Replace with the actual recipient's email
        GITHUB_REPO = 'git@github.com:DKallans/jenkins-trial.git' // Replace with your repo URL
        MAIN_BRANCH = 'main' // Your main branch name
    }
    stages {
        stage('Checkout') {
            steps {
                //Check out the code from the GitHub repository
                checkout scmGit(branches: [[name: '*/ft-setup']], extensions: [], userRemoteConfigs: [[url: 'git@github.com:DKallans/jenkins-trial.git']])
            }
        }
        
        stage('Build and Test') {
            steps {
                script {
                    // Run Maven build and tests
                    bat 'mvn clean install'
                }
            }
        }

        stage('Merge to Main') {
            steps {
                script {
                    // Ensure we are on the main branch and update it
                    bat 'git checkout main'
                    bat 'git pull origin main'
                    
                    // Merge the feature branch into the main branch
                    bat 'git merge origin/${env.BRANCH_NAME}'
                    
                    // Push changes back to the main branch
                    bat 'git push origin main'
                }
            }
        }

        stage('Package WAR') {
            steps {
                script {
                    // Package the WAR file using Maven
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Send Email with WAR Attachment') {
            steps {
                script {
                    // Use the Jenkins Email Extension Plugin to send the email with the WAR file
                    emailext (
                        subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The build has been successfully completed, and the WAR file is attached.",
                        to: "${env.EMAIL_RECIPIENT}",
                        attachLog: true,
                        attachmentsPattern: WAR_FILE
                    )
                }
            }
        }
    }
    post {
        always {
            // Clean up workspace
            cleanWs()
        }
        success {
            echo "Build and deployment completed successfully!"
        }
        failure {
            echo "Build failed. Check the logs for details."
        }
    }
}
