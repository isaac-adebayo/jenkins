# Jenkins

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
