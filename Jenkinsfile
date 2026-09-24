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
        /*stage('Deploy') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    bat '''
                    scp -o StrictHostKeyChecking=no target/*.jar ec2-user@3.110.164.176:/home/ec2-user/app/
                    '''
                }
            }
        }
        */
        stage('Deploy') {
             steps {
                sshagent(['ec2-ssh-key']) {
                     bat '''
                    scp -o StrictHostKeyChecking=no target\\*.jar ec2-user@3.110.164.176:/home/ec2-user/app/

                     ssh -o StrictHostKeyChecking=no ec2-user@3.110.164.176 "PID=$(pgrep -f '/home/ec2-user/app/devops-app-0.0.1-SNAPSHOT.jar' | head -n 1); if [ -n \\"$PID\\" ]; then kill $PID; fi; nohup java -jar /home/ec2-user/app/devops-app-0.0.1-SNAPSHOT.jar > /home/ec2-user/app/app.log 2>&1 &"
                    '''
                 }
            }
        }
    }
}
