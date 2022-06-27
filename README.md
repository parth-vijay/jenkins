# Install Jenkins in ubuntu

## Install Java:
    sudo apt update 
    sudo apt install openjdk-8-jdk

## Install Jenkins:
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    sudo apt update
    sudo apt install jenkins

## Start/Stop/Status Jenkins:
    sudo systemctl start jenkins
    sudo systemctl stop jenkins
    sudo systemctl status jenkins

## Opening firewall:
    sudo ufw allow 8080
    sudo ufw status

# Kubernetes deployment with declarative pipeline in Jenkins

Ensure that you installed docker engine on Jenkins instance and git, docker plugins.
Install Docker-CE on Ubuntu 18 from https://docs.docker.com/engine/install/ubuntu/

## Allow Jenkins users to access the docker socket:
    sudo usermod -aG docker jenkins
    sudo systemctl restart jenkins

## Create credentials on Jenkins for GitHub and docker hub:

1. To create a GitHub credentials go to **Manage Jenkins->Manage Credentials(Under Security)** click to **Jenkins Store**.
2. Then click to **Global credentials**.
3. Click **Add Credentials** on the left menu.
4. Choose **Username and Password**.
5. **Username** is the GitHub user ID and **Password** is the personal API Token.
6. Create another user and password credentials for **Docker Hub** login.

## Create credentials on Jenkins for Kubernetes config file:

1. Go to **Manage Jenkins->Manage Credentials(Under Security)** click to **Jenkins Store**.
2. Then click to **Global credentials**.
3. Click **Add Credentials** on the left menu.
4. Choose **Secret file**.
5. Choose **kubeconfig** file from your system.
6. Give **ID** name and **Description**.
7. Click **OK**.

## Create Webhook on Github:

1. Go to the **Settings->Webhook** in your repo and add.
2. Type your **https://jenkinsURL/github-webhook/**.  (Don’t forget to put / at the end of the URL)
3. Choose Content-Type as **application/json and** choose **Let me select individual events** select **Pull Requests and Pushes**.

Whenever you do a pull or push in the repo, Github will inform the Jenkins. Ensure that your Jenkins URL is accessible from Github.

## Configure Jenkins Pipeline:

1. Click on **New item**.
2. Give a name and select **Pipeline**.
3. Enable **GitHub hook trigger for GITScm polling at Build Triggers**
4. Choose Definition **Pipeline script from SCM**.
   **SCM: Git**
5. Credentials: The one we created earlier. You can create the credentials in this section too.
6. Branch to build: This is just test build I chose the master origin/master
7. Script Path: Where you keep Jenkinsfile. We keep it in the repo base directory. If this is in the subpath, please specify here.

Just update something in the repo and push the changes.