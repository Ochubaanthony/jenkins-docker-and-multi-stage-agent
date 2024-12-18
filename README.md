# jenkins-docker-and-multi-stage-agent

JENKINS 

- Create a Server on AWS OR AZURE VM
- Cd downloads
- Chmod 600 tony-key.pem 
  -   shh -I Tony-key.pem ubuntu@1234566
- sudo apt update && sudo apt upgrade -y

RUN THE BELOW COMMANDS TO INSTALL JAVA AND JENKINS
Install Java

sudo apt update
sudo apt install openjdk-17-jre   

OPTIONAL
sudo apt install maven -y

VERIFY JAVA IS INSTALLED

Java -version

Now, you can proceed with installing Jenkins

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

COPY THE FILE FROM THE JENKINS
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Click on INSTALLED SUGESTED PLUGIN
Enter Username
Enter Password
Fullname
Email
Save and continue 
Save and Continue 


USE
https://github.com/vdespa/learn-jenkins-app

Steps
Install the Docker Pipeline plugin in Jenkins
* Log in to Jenkins.
* Go to Manage Jenkins > Manage Plugins.
* In the Available tab, search for "Docker Pipeline".
* Select the plugin and click the Install button.
* Restart Jenkins after the plugin is installed.
Wait for the Jenkins to be restarted.



How it Works
1. Jenkins starts the pipeline and creates a Docker container using the node:16-alpine image.
2. Inside the container:
    * The shell command node --version runs to print the version of Node.js.
3. After the stage completes, the container is terminated unless configured otherwise.

pipeline {
  agent {
    docker { image 'node:16-alpine' }
  }
  stages {
    stage('Test') {
      steps {
        sh 'node --version'
      }
    }
  }
}




WITHOUT DOCKER 

pipeline {
    agent any

    stages {
        stage('Without Docker') {
            steps {
                script {
                    echo 'Without Docker'
                }
            }
        }
    }
}


MULTI-STAGE-MULTI-AGENT

pipeline {
  agent none
  stages {
    stage('Back-end') {
      agent {
        docker { image 'maven:3.8.1-adoptopenjdk-11' }
      }
      steps {
        sh 'mvn --version'
      }
    }
    stage('Front-end') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        sh 'node --version'
      }
    }
  }
}

WITH DOCKER AND WITHOUT DOCKER

pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
                sh 'echo "Without docker"'
            }
        }
         
        stage('w/ docker') {
            agent {
                docker {
                    image 'node:18-alpine'
                }
            }
            steps {
                sh 'echo "With docker"'
            }
        }
    }
}


GO TO THE SERVER AND RUN THIS BELOW
Docker Slave Configuration
sudo apt update
sudo apt install docker.io

Grant Jenkins user and Ubuntu user permission to docker deamon.
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
sudo usermod -aG docker $USER
newgrp docker


Log out and log back in: After running the command, log out and log back in, or you can use the following command to apply the changes without logging out:
newgrp docker

Verify Docker access: Try running a Docker command, like:
docker run hello-world


Once you are done with the above steps, it is better to restart Jenkins.
http://<ec2-instance-public-ip>:8080/restart
The docker agent configuration is now successful.
