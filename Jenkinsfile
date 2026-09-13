pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk-17'
    }

    stages {

        stage('Checkout') {
            steps {
                git(
                    branch: 'main',
                    url: 'https://github.com/Irfaanpk/MavenApp-deployment-to-Ec2-with-JenkinsPipeline.git'
                )
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['deploy-creds']) {
                    bat '''
                        scp -o StrictHostKeyChecking=no target\\demo-1.0.0.jar ubuntu@3.91.55.4:/opt/app/

                        ssh -o StrictHostKeyChecking=no ubuntu@3.91.55.4 "pkill -f demo-1.0.0.jar || true"

                        ssh -o StrictHostKeyChecking=no ubuntu@3.91.55.4 "nohup java -jar /opt/app/demo-1.0.0.jar > /opt/app/app.log 2>&1 &"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully.'
        }

        failure {
            echo 'Deployment failed. Check Jenkins or EC2 logs.'
        }
    }
}
