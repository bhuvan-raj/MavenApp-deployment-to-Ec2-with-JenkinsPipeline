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
                    sh '''
                        echo "Copying artifact to EC2..."
                        scp -o StrictHostKeyChecking=no target/demo-1.0.0.jar ubuntu@34.224.84.252:/opt/app/

                        echo "Restarting application on EC2..."
                        ssh -o StrictHostKeyChecking=no ubuntu@34.224.84.252 "bash -lc '
                            pkill -f demo-1.0.0.jar || true
                            nohup java -jar /opt/app/demo-1.0.0.jar > /opt/app/app.log 2>&1 &
                        '"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
        }
        failure {
            echo "Deployment failed. Please check Jenkins logs and EC2 application logs."
        }
    }
}
