pipeline {
    agent any

    tools {
        maven 'maven-3.9'
        jdk 'jdk-17'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-org/demo-app.git'
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
                    scp -o StrictHostKeyChecking=no target/demo-1.0.0.jar ec2-user@<EC2_PUBLIC_IP>:/opt/app/
                    ssh -o StrictHostKeyChecking=no ec2-user@<EC2_PUBLIC_IP> '
                        pkill -f demo-1.0.0.jar || true
                        nohup java -jar /opt/app/demo-1.0.0.jar > app.log 2>&1 &
                    '
                    """
                }
            }
        }
    }
}
