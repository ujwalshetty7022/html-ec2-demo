pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "myhtmlapp"
        EC2_USER = "ec2-user"
        EC2_HOST = "18.232.147.134"          
        SSH_CREDENTIALS = "ujwal_key_pem"        
        GIT_REPO = "https://github.com/ujwalshetty7022/html-ec2-demo.git"   
        GIT_BRANCH = "main"
    }

    stages {

        stage("Checkout Code") {
            steps {
                git branch: "${GIT_BRANCH}", url: "${GIT_REPO}"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh """
                docker build -t ${IMAGE_NAME} .
                """
            }
        }

        stage("Deploy to EC2") {
            steps {
                sshagent(["${SSH_CREDENTIALS}"]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                        docker rm -f ${IMAGE_NAME} || true
                        docker run -d --name ${IMAGE_NAME} -p 80:80 ${IMAGE_NAME}
                    '
                    """
                }
            }
        }
    }
}
