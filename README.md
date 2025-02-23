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
4. Copy the the 'HTTPS' repository URL from the github web UI.
5. Go to the 'Source Code Management' tab under project's configuration menu in the jenkins web UI.
6. Check the 'Git' radio button and paste the copied repository URL in the 'Repository URL' textbox
7. For 'Branch Specifier', select '*/main' option
8. Save configuration and run 'Build Now' to connect jenkins to the repository.

### Automating buils with 'Build Triggers' in jenkins using github webhook
---
1. Github weebhook can be configured to send trigger to jenkins whenever there is a change in the repository contents
2. Click the 'Configure' menu of the project and click the 'Build Triggers'
3. Check the 'Github hook trigger for GITScm polling'
4. Go to the 'Settings' option in the github repository and select 'Webhooks', paste the jenkins server's IP address into the 'Payload URL' textbox of the webhook and append '/github-webhook/' to the pasted IP address, e.g **'https://127.0.0.1:8080/github-webhook/'**
5. Select 'application/json' for 'Content type'. Select other options as appriopriate
6. Click 'Add webhook'
7. <u>**Note:**</u>
   **_SCM polling_** can be selected for making jenkins to poll scm directory instead of making use of webhook

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
