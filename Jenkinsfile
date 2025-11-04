pipeline {
    agent {
        docker { image 'docker:latest' args '-v /var/run/docker.sock:/var/run/docker.sock' }
    }

    environment {
        EC2_USER = "ubuntu"
        EC2_HOST = "18.232.147.134"   
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/ujwalshetty7022/html-ec2-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t myhtmlapp ."
            }
        }

        stage('Test HTML Exists') {
            steps {
                sh "test -f index.html"
                echo "HTML file exists"
            }
        }

        stage('Save Docker Image') {
            steps {
                sh "docker save myhtmlapp -o myhtmlapp.tar"
            }
        }
        
        stage('Copy image to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh """
                    scp -o StrictHostKeyChecking=no myhtmlapp.tar $EC2_USER@$EC2_HOST:/home/ubuntu/
                    """
                }
            }
        }

        stage('Run Docker on EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no $EC2_USER@$EC2_HOST '
                        docker load -i myhtmlapp.tar &&
                        docker stop myhtmlapp || true &&
                        docker rm myhtmlapp || true &&
                        docker run -d --name myhtmlapp -p 80:80 myhtmlapp
                    '
                    """
                }
            }
        }
    }
}
