pipeline {
    agent any

    tools {
        maven 'maven-3.9'
        jdk 'JDK17'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/bhuvan-raj/MavenApp-deployment-to-Ec2-with-JenkinsPipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh """
                    scp -o StrictHostKeyChecking=no target/demo-1.0.0.jar ubuntu@34.224.84.252:/opt/app/
                    ssh -o StrictHostKeyChecking=no ubuntu@34.224.84.252 '
                        pkill -f demo-1.0.0.jar || true
                        nohup java -jar /opt/app/demo-1.0.0.jar > app.log 2>&1 &
                    '
                    """
                }
            }
        }
    }
}
