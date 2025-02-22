# Jenkins

### Jenkins installation
---
1. After installation of jenkins through the terminal, open jenkins on the port 8080 of the server's IP address e.g 127.0.0.1:8080
2. Use the default password to login
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
3. Install the suggested plugins and create first admin user. Input username, password, email etc.<br>**Note:** The username and password created here will be used to login to the server onwards.

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
