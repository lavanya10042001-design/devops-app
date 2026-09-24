pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Test'){
            steps {
                bat 'mvn test'
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    bat '''
                    scp -o StrictHostKeyChecking=no target/*.jar ec2-user@3.110.164.176:/home/ec2-user/app/
                    '''
                }
            }
        }
    }
}
