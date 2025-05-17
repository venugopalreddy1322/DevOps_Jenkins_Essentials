# Jenkins Notes:


## Jenkins Setup & Configuration
## Install the necessary softwares
- **Jenkins**
  - Go to official Jenkins website to Install Jenkins  
- **Terraform Configuration**  
  - Navigate to **Manage Jenkins** → **Tools** → **Terraform**  
  - Uncheck **Install automatically**  
  - Set installation directory to `which terraform` output (e.g., `/usr/bin/terraform`)

## Installed Plugins for the demos
- **Docker**
- **Terraform Configuration**  
  - Navigate to **Manage Jenkins** → **Tools** → **Terraform**  
  - Uncheck **Install automatically**  
  - Set installation directory to `which terraform` output (e.g., `/usr/bin/terraform`)

## Global Credentials Added
- **GitHub**  
  - Username: `<your GitHub username>` (e.g., `venugopalreddy1322`)  
  - Personal access token: _Settings → Developer Settings → Personal Token_
- **DockerHub**  
  - Username: _same as DockerHub login_  
  - Password: **DockerHub password**

## Connecting a Vagrant Machine to AWS
1. Configure AWS credentials on the Vagrant machine:
   ```sh
   aws configure
   ```
2. Jenkins user is created automatically during installation.
3. Add Jenkins user to the sudo group:
   ```sh
   sudo usermod -aG sudo jenkins
   ```

## Setting Up AWS Credentials for Jenkins
1. Create the `.aws` directory inside the Jenkins home directory:
   ```sh
   sudo mkdir -p /var/lib/jenkins/.aws
   ```
2. Copy AWS credentials file from Vagrant machine:
   ```sh
   sudo cp /home/vagrant/.aws/credentials /var/lib/jenkins/.aws/credentials
   ```
3. Set the correct ownership and permissions:
   ```sh
   sudo chown -R jenkins:jenkins /var/lib/jenkins/.aws
   sudo chmod 600 /var/lib/jenkins/.aws/credentials
   ```

## Terraform Note
When running Terraform commands, using `cd myfolder` alone will return to the root directory. Instead, use:
In terraform 
```sh 
sh 'cd myfolder'
```
will not continue to the next line it will come back to the root directory. so:
1. Use sh """  ... """ for multiple commands like:
 ```sh 
sh """
        cd myfolder
        terraform init
        terraform plan
        terraform apply --auto-approve
    """
```
2. Or use
```sh
sh 'cd myfolder; terraform init; terraform plan; terraform apply --auto-approve'
 ```
3. Or Use:  
```sh
cd myfolder && terraform init && terraform plan && terraform apply --auto-approve
```
4. Alternatively, In fact best way is to use Jenkins `dir()` step:
```groovy
steps {
    dir('myfolder') {
        // Terraform commands here
    }
}
```
-----------------------------------------------------------------------
## Changing Jenkins Default Port (8080 → 8081)
### Step 1: Modify Configuration File
1. Open the Jenkins configuration file:
   ```sh
   sudo nano /etc/default/jenkins
   ```
2. Locate and update the port:
   ```sh
   JENKINS_PORT=8081
   ```
3. Save and exit (`Ctrl + X`, then `Y`, then `Enter`).
4. Restart Jenkins:
   ```sh
   sudo systemctl restart jenkins
   ```

### Step 2: Modify Systemd Service File
1. Open the Jenkins systemd service file:
   ```sh
   sudo nano /usr/lib/systemd/system/jenkins.service
   ```
2. Locate and update the environment variable:
   ```sh
   Environment="JENKINS_PORT=8081"
   ```
3. Save and exit (`Ctrl + X`, then `Y`, then `Enter`).
4. Reload systemd daemon:
   ```sh
   sudo systemctl daemon-reload
   ```
5. Restart Jenkins:
   ```sh
   sudo systemctl restart jenkins
   ```
6. Verify if another process is using port 8081:
   ```sh
   sudo netstat -tulnp | grep 8081
   ```

---


```
