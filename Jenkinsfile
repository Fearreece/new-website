pipeline{
    agent any
    stages{
        stage("Login stage"){
            steps{
                echo 'Logging into EC2 instance'
                sh '''
                sudo ssh -i /home/ubuntu/express.pem -t -o StrictHostKeyChecking=no ubuntu@ec2-44-202-248-220.compute-1.amazonaws.com
                sudo apt-get update
                '''

            }
        }
        stage("installing Docker"){
            steps{
                sh '''
                sudo apt-get install \
                ca-certificates \
                curl \
                gnupg \
                lsb-release -y
                sudo mkdir -p /etc/apt/keyrings
                "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
                $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
                sudo apt-get update
                sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
                '''

            }
        }
        stage("pulling image"){
            steps{
                sh '''
                cd /var/www
                sudo rm -rf html
                sudo mkdir html
                cd html
                sudo git init
                sudo git remote add origin http://github.com/Fearreece/new-website.git
                sudo git pull origin main
            
                '''
                
            }
        }
        stage('Building image'){
            steps{
                echo 'Your image is building'
                sh '''
                    sudo docker build -t express:latest .
                    sudo docker run --name express -d -p 3000:3000 express:latest
                '''
                
            }
        }
    }
}