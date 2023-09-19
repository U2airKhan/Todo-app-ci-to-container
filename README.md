# Todo-app-ci-to-container

Simple and easy step for this Jenkins declarative pipline project..

*Step1: You have to launch an simple Ec2-instance with ubuntu AMI.

*Step2: SHH to that Ec2-Instance using keypair that you created while making instance.
    
     example: ssh -i "your-key-directory/key-name" "instance-username@instance-public-ip"

*Step3: Install jenkins in that instance
     
     sudo apt update
     sudo apt install openjdk-11-jre
     java -version
     curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
      /usr/share/keyrings/jenkins-keyring.asc > /dev/null
     echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
      https://pkg.jenkins.io/debian binary/ | sudo tee \
      /etc/apt/sources.list.d/jenkins.list > /dev/null
     sudo apt-get update
     sudo apt-get install jenkins

*Step4: Start jenkins
     
     sudo systemctl enable jenkins
     sudo systemctl start jenkins
     sudo systemctl status jenkins

*Step5: Install docker
     
     sudo apt-get install docker.io

*Step6: Add user user into docker group and provide user permissions to the docker.sock file
     
     jenkins --version
     docker --version
     sudo usermod -a -G docker $USER
     sudo chmod 666 /var/run/docker.sock

*Step7: Change some network settings of Ec2-Instance from Security Group
  
    1: allow 8080 from MyIp.
    2: allow 8000 from anywhere
    3: allow ssh from anywhere or myip. both works fine

*Step8: Browse Jenkins server.
  
    copy your ec2-instance Public-ip:8000 and paste it in the browser
    unlock jenkins password using sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    make some registration settings alongs with some suggestive pluggins installation process
    Create a new pipline project from jenkins dashboard

*Step9: Configure project and write pipline script given below
 
    pipeline {
      agent any
    
      stages {
          stage('Fetch Source Code') {
             steps {
                git url: "https://github.com/U2airKhan/Todo-app-ci-to-container.git", branch: "main"
             }
          }
          stage('Build Image') {
            steps {
               sh " docker build -t todo-ci-app ."
            }
          }
          stage('Launch Application Container') {
            steps {
                sh "docker run -d --name todo-app -p 8000:8000 todo-ci-app"
            }
          }
      }
    }

*Step10: Save and build pipline.

*Step11: If pipline successfully completed then SSH to your instance
 
    check your container is up and running by command " docker ps ".
    copy your public-ip along with port that is used in mapping container app which is 8000 in this case you can check it in above last stage.
    paste public-ip:8000 to the browser and enjoy todo-app.


******************** best of luck for your project and keep learning **************************

