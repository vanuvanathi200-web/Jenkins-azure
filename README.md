1.install jenkins

EXPERIMENT: INSTALL JENKINS ON UBUNTU
AIM:

To install and configure Jenkins on Ubuntu operating system.

PROCEDURE:

Step 1: Update the package list
sudo apt update


Step 2: Install Java
sudo apt install fontconfig openjdk-17-jre -y


Check Java version:
java -version


Step 3: Add Jenkins Repository Key
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key


Step 4: Add Jenkins Repository
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null


Step 5: Update Package List
sudo apt update


Step 6: Install Jenkins
sudo apt install jenkins -y


Step 7: Start Jenkins Service
sudo systemctl start jenkins


Step 8: Enable Jenkins
sudo systemctl enable jenkins


Step 9: Check Jenkins Status
sudo systemctl status jenkins


Step 10: Open Jenkins in Browser

Enter the following URL:
http://localhost:8080


Step 11: Unlock Jenkins

Get the initial administrator password:
sudo cat /var/lib/jenkins/secrets/initialAdminPassword


- Copy the password.
- Paste it into the Jenkins Unlock page.
- Click Continue.
- Select Install suggested plugins.
- Create an admin username and password.
RESULT:

Thus, Jenkins was successfully installed and configured on Ubuntu.

----------------------------------------------------------------------------------------------------------------------------------------------------------------
2.periodic build in jenkins

Experiment 1(b) – Jenkins Periodic Build
Step 1 – Create a directory
Ubuntu Terminal open panni:
mkdir jenkins_lab


Step 2 – Enter into the directory
cd jenkins_lab


Step 3 – Create a file using Nano Editor
nano jen.txt


Nano editor open aagum. Adhula type pannu:
Hello Jenkins


Save panna:
- Ctrl + O press pannu.
- Enter press pannu.
- Ctrl + X press panni exit pannu.
Step 4 – Display the file content
cat jen.txt


Output:
Hello Jenkins


Step 5 – Set file permissions
sudo chmod 755 /home/pavi


sudo chmod 755 /home/pavi/jenkins_lab


sudo chmod 644 /home/pavi/jenkins_lab/jen.txt


Step 6 – Open Jenkins Dashboard
Browser-la:
http://localhost:8080


1. Click New Item.
2. Enter name: Periodic-Build.
3. Select Freestyle Project.
4. Click OK.
Step 7 – Configure Periodic Build
Build Triggers section-la:
- Tick Build periodically.
- Schedule box-la enter:
* * * * *


Idhu every one minute-ku build trigger pannum.
Step 8 – Configure Execute Shell
Build Steps → Add build step → Execute shell.

Enter:
echo "Periodic Build Started"
date
echo "File Content:"
cat /home/pavi/jenkins_lab/jen.txt


Click Save.
Step 9 – Run the Build
1. Click Build Now.
2. Open Build History.
3. Click the build number.
4. Click Console Output.
Expected Output:
Periodic Build Started
File Content:
Hello Jenkins
Finished: SUCCESS


Step 10 – Modify the file using Nano
Ubuntu Terminal open panni:
nano /home/pavi/jenkins_lab/jen.txt


Old content-ah change panni:
Hello Jenkins - File Updated


Save:
- Ctrl + O
- Enter
- Ctrl + X
Step 11 – Verify modified file
cat /home/pavi/jenkins_lab/jen.txt


Output:
Hello Jenkins - File Updated


Step 12 – Verify Periodic Build
Next one-minute periodic build trigger aagattum.

Jenkins → Periodic-Build → Latest Build → Console Output.

Expected Output:
Periodic Build Started
File Content:
Hello Jenkins - File Updated
Finished: SUCCESS



Result
Jenkins was installed successfully. A text file was created using Nano Editor, periodic build was configured, and modifications made to the file were successfully reflected in the Jenkins build.

------------------------------------------------------------------------------------------------------------------------------------------------------------------
3.CI pipeline

CREATE CI PIPELINE USING JENKINS
AIM:

To create a CI pipeline using Jenkins.

PROCEDURE:

Step 1: Fork GitHub Repository
- Open the GitHub repository: https://github.com/benc-uk/python-demoapp.git
- Click Fork to copy it to your GitHub account.
Step 2: Modify Repository Files
- Open src/requirements.txt and make the required changes.
- Open build/Dockerfile and replace the GitHub repository link with your own repository link.
- Open Makefile and replace the GitHub username with your own username and ghcr.io with docker.io.
Step 3: Create Jenkins Job
- Open Jenkins (http://localhost:8080).
- Click New Item.
- Enter the name Install_tools.
- Select Pipeline and click OK.
Step 4: Configure Install Tools Pipeline

- Select Pipeline Script and enter the following code:
pipeline {
    agent any
    stages {
        stage('Python Version') {
            steps {
                sh "sudo apt update"
                sh "sudo apt install python3-venv -y"
                sh "python3 --version"
            }
        }
        stage('Docker Version') {
            steps {
                sh "docker -v"
            }
        }
        stage('Install Make') {
            steps {
                sh "sudo apt install make -y"
            }
        }
    }
}


- Click Save and Build Now.
- Check Console Output for Finished: SUCCESS.
Step 5: Install Docker Plugins
- Go to Manage Jenkins → Plugins → Available Plugins.
- Search and install the required Docker plugins.
- Install Docker using Ubuntu terminal:
sudo apt install docker.io -y
sudo groupadd docker
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins


Step 6: Configure Docker
- Go to Manage Jenkins → Tools.
- Configure Docker installation.
- Create an account in Docker Hub.
Step 7: Create CI Pipeline Job
- Go to Jenkins Dashboard → New Item.
- Enter the name python-webapp.
- Select Pipeline and click OK.
- Select Discard old builds and set maximum builds to 1.
Step 8: Configure CI Pipeline Script
- Go to Pipeline → Pipeline Script.
- Enter the following code:
pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git url: 'YOUR_GITHUB_REPOSITORY_URL',
                    branch: 'main'
            }
        }

        stage('Docker Build') {
            steps {
                sh "make image"
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    withDockerRegistry(
                        credentialsId: 'YOUR_CREDENTIAL_ID',
                        toolName: 'YOUR_DOCKER_TOOL'
                    ) {
                        sh "make push"
                    }
                }
            }
        }
    }
}


Step 9: Add Credentials
- Open Pipeline Syntax → Snippet Generator.
- Generate the Git checkout script using your GitHub repository URL and branch.
- Add Docker Hub username and password under Manage Jenkins → Credentials.
- Generate the Docker Registry snippet and copy the credentials ID and Docker tool name into the pipeline script.
Step 10: Build and Verify
- Click Save.
- Select Build Now.
- Open Console Output and verify the execution.
- Check whether the Docker image is pushed successfully to Docker Hub.
RESULT:

Thus, the CI pipeline was successfully created and executed using Jenkins.

------------------------------------------------------------------------------------------------------------------------------------------------------------------
