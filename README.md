# Jenkins

### Jenkins installation
---
1. After installation of jenkins through the terminal, open jenkins on the port 8080 of the server's IP address e.g 127.0.0.1:8080
2. Use the default password to login
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
3. Install the suggested plugins and create first admin user. Input username, password, email etc.<br>**Note:** The username and password created here will be used to login to the server onwards.

### Connecting Freestyle job to Github
---
1. Click on 'New Item' in the jenkins' web UI, input the name of the project and select 'Freestyle project'.
2. Create a new github repository called jenkins-scm with a README.md file
3. Connect **'jenkins'** to the **'jenkins-scm'** repository created. On github, ensure you are in the **'main'** branch.
4. Copy the the 'HTTPS' repository URL from the github web UI. E.g https://github.com/isaac-adebayo/jenkins-scm.git
5. Go to the 'Source Code Management' tab under project's configuration menu in the jenkins web UI.
6. Check the 'Git' radio button and paste the copied repository URL in the 'Repository URL' textbox
7. For 'Branch Specifier', select '*/main' option
8. Save configuration and run 'Build Now' to connect jenkins to the repository.

### Jenkins Pipeline job - example
---
Jenkins pipeline can be set up using jenkins file which can contain either a declarative pipeline or a scripted pipeline.

1. Click the 'New Item' in the jenkins web UI, enter the job name, select 'Pipeline' option and click 'Ok'
2. **Configure the Github's project URL, Trigger and the Pipeline:** Go to the 'Configure' option of the pipeline job
3. In the 'General' tab check the 'Github project' checkbox and enter the github's project URL (Do this if the jenkins server has not be previously linked to Github in the 'Freestyle job'
4.  In the 'Triggers' tab, check the 'Github hook trigger for GITScm polling' checkbox. Otherwise use 'Poll SCM' for local test environment
5.  In the 'Pipeline' tab, pipeline script for build steps for the job can be defined: The Groovy script used to define the pipeline is of the below format:
    ```
    pipeline {
      agent any
      stages
      {
        stage...
        stage...
        ...
      }
    }
    
   - The **'checkout'** step is used to fetch the source code from GitHub into the Jenkins workspace, so the build process can work with the latest code:
     ```
     stage('Connect To Github')
     {
       steps 
       {
          checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/username/repository.git']])
       }
     }
     ```
  - Build steps involving running shell commands can be implemented using **'sh'** step. E.g
    ```
    stage('Build Docker Image')
    {
      steps
      {
        sh 'docker build -t dockerfile .'
      }
    }
    ```
**Note:** 
1. Each steps can be generated using the 'Pipeline Syntax' option in the pipeline tab
2. Some errors encountered and resolution:
   ### ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker
   - Create docker group if not exists
     ```
     sudo groupadd docker
     ```
   - Add existing user to the docker group
     ```
     sudo usermod -aG docker $USER
     ```
   - Login back into docker or use the 'newgrp' command to reinitialise the docker group
     ```
     newgrp docker
     ```
   ### sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
   - Configure sudo to Allow Jenkins User to Run Docker Commands Without a Password
   - Open the Sudoers File:
     ```
     sudo visudo
     ```
   - Add the Following Line:
     ```
     jenkins ALL=(ALL) NOPASSWD: /usr/bin/docker
     ```
   - Save & exit
  

### Automating buils with 'Build Triggers' in jenkins using github webhook
---
1. Github weebhook can be configured to send trigger to jenkins whenever there is a change in the repository contents
2. Click the 'Configure' menu of the project and click the 'Build Triggers'
3. Check the 'Github hook trigger for GITScm polling'
4. Go to the 'Settings' option in the github repository and select 'Webhooks', paste the jenkins server's IP address into the 'Payload URL' textbox of the webhook and append '/github-webhook/' to the pasted IP address, e.g **'https://127.0.0.1:8080/github-webhook/'**
5. Select 'application/json' for 'Content type'. Select other options as appriopriate
6. Click 'Add webhook'
7. **Note:**<br>
   **_Poll SCM_** can be selected for making jenkins to poll scm directory in a test environment instead of making use of webhook

### Reset Jenkins
---
In case the login details of the jenkins server is forgotten, the jenkins configuration can be reset to the default in the terminal.

**_1. Stop the jenkins server_**
```
sudo systemctl stop jenkins
```
**_2. Backup configuration files_**
```
sudo cp -r /var/lib/jenkins /var/lib/jenkins_backup
```
**_3. Remove the configuration files to reset the jenkins to its default state_**
```
sudo rm -rf /var/lib/jenkins/*
```
**_4. Restart the jenkins server_**
```
sudo systemctl start jenkins
```
