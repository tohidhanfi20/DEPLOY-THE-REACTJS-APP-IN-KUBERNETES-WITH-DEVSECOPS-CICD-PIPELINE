# Getting Started with 2048

This game (2048) was built using **React** and **TypeScript**. The unique part of this example is animations. The animations in React aren't that straightforward, so I hope you can learn something new from it.


# STEP 1 : Launch an Ubuntu(22.04) T2 Large Instance

image

# Step 2 —

# 2A - Install Jenkins

     sudo apt update -y

     sudo apt upgrade -y 

     sudo apt install openjdk-17-jre -y

     curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
     /usr/share/keyrings/jenkins-keyring.asc > /dev/null
     echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
     https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
     /etc/apt/sources.list.d/jenkins.list > /dev/null
     sudo apt-get update -y 
     sudo apt-get install jenkins -y


Go to AWS EC2 Security Group and open Inbound Port 8080 and in
server get the password

<EC2 Public IP Address:8080>

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

image

UNLOCK JENKINS

image

Create a user click on save and continue.

image

# 2B — Install Docker and setup Sonarqube

    sudo apt update -y

    sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" -y

    sudo apt update -y

    apt-cache policy docker-ce -y

    sudo apt install docker-ce -y

    #sudo systemctl status docker
 
    sudo chmod 777 /var/run/docker.sock

After the docker installation, we created a Sonarqube container
(Remember to add 9000 ports in the security group).

    docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

image

Now our Sonarqube is up and running

image

Enter username and password, click on login and change password

username admin / password admin

Update New password, This is Sonar Dashboard.

image

# 2C — Install Trivy [ IAC ]

     sudo apt-get install wget apt-transport-https gnupg lsb-release
     wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
     echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
     sudo apt-get update
     sudo apt-get install trivy

Step 3 — Install Plugins like JDK, Sonarqube Scanner,
NodeJS, OWASP Dependency Check

# 3A — Install Plugin

Go to Manage Jenkins →Plugins → Available Plugins →
Install below plugins
1 → Eclipse Temurin Installer (Install without restart)

2 → Sonarqube Scanner (Install without restart)

3 → NodeJS Plugin (Install Without restart)

# 3B — Configure Java and Nodejs in Global Tool Configuration

Goto Manage Jenkins → Tools → Install JDK(17) and NodeJs(16)→
Click on Apply and Save

image

# 3C — Create a Job

create a job as Devsecops_demo Name, select pipeline and click ok.

# Step 4 — Configure Sonar Server in Manage Jenkins

Grab the Public IP Address of your EC2 Instance, Sonarqube works on
Port 9000, so <Public IP>:9000.

Goto your Sonarqube Server.

Click on Administration → Security → Users → Click on Tokens and
Update Token → Give it a name → and click on Generate Token

image

image

copy Token

Goto Jenkins Dashboard → Manage Jenkins → Credentials → Add Secret
Text. It should look like this

image

image

Now, go to Dashboard → Manage Jenkins → System and Add like the
below image.

image

Click on Apply and Save

The Configure System option is used in Jenkins to configure different
server

Global Tool Configuration is used to configure different tools that we
install using Plugins

We will install a sonar scanner in the tools.

image

In the Sonarqube Dashboard add a quality gate also

Administration → Configuration →Webhooks

image

image

Add details

image

image

Let’s go to our Pipeline and add the script in our Pipeline Script.

